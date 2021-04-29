---
layout: post
title: set, detach, flush and refresh methods using JPA/hibernate
---

A short blog on what effect the **set**, **detach** and **refresh** methods have in the database.
If you want to play and try your self, follow the set up instructions [here](https://konstantinosblatsoukasrepo.github.io/jpa-orm-setup/).

In order to see what query is executed in the database, in the application.properties file
add the following line:

```sh
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

## set method

When a set method is executed on an entity, then an **update** operation is executed.

Example:

```java
// connect to DB
// EntityManager: manages the entities, is an interface to PersistenceContext
@PersistenceContext
EntityManager entityManager;

public void updateFirstName(int employeeId) {
    entityManager.getTransaction().begin();
    Employee employee = entityManager.find(Employee.class, employeeId);
    employee.setFirstName("Kostas");
    entityManager.getTransaction().commit();
}
```

queries performed:

```sql
    select
        employee0_.emp_no as emp_no1_2_0_,
        employee0_.birth_date as birth_da2_2_0_,
        employee0_.first_name as first_na3_2_0_,
        employee0_.gender as gender4_2_0_,
        employee0_.hire_date as hire_dat5_2_0_,
        employee0_.last_name as last_nam6_2_0_
    from
        employees employee0_
    where
        employee0_.emp_no=?

    update
        employees
    set
        birth_date=?,
        first_name=?,
        gender=?,
        hire_date=?,
        last_name=?
    where
        emp_no=?
```

One select is performed for retrieving the entity and one update due to set.

## detach method

By executing the detach method in an entity, this entity is not longer in the persistent context
(e.g. it has no effect in the database, is like a POJO).

Example:

```java
public void playWithDetach(int employeeId) {
    entityManager.getTransaction().begin();
    Employee employee = entityManager.find(Employee.class, employeeId);
    entityManager.detach(employee);
    employee.setFirstName("Kostas");
    entityManager.getTransaction().commit();
}
```

queries performed:

```sql
    select
        employee0_.emp_no as emp_no1_2_0_,
        employee0_.birth_date as birth_da2_2_0_,
        employee0_.first_name as first_na3_2_0_,
        employee0_.gender as gender4_2_0_,
        employee0_.hire_date as hire_dat5_2_0_,
        employee0_.last_name as last_nam6_2_0_
    from
        employees employee0_
    where
        employee0_.emp_no=?
```

Only one select was performed the set operation was ignored (detach did the job).

## flush and refresh

By using the refresh method in an entity, you get the latest state from the database.
Flush method, forces actions to be performed immediately and not to end of the transaction
(e.g. when the commit is performed, to be precise this can be configured through the [FlushModeType](https://docs.oracle.com/javaee/5/api/javax/persistence/FlushModeType.html)).

Example:

```java
public void playWithEntityManagerFlushAndRefresh(int employeeId) {
    Employee employee = entityManager.find(Employee.class, employeeId);
    employee.setFirstName("Messi");

    entityManager.flush();
    // after the refresh, the value of the first name must be Messi
    entityManager.refresh(employee);
    System.out.println(employee.getFirstName());
}
```

queries performed/output:

```sql
Hibernate:
    select
        employee0_.emp_no as emp_no1_2_0_,
        employee0_.birth_date as birth_da2_2_0_,
        employee0_.first_name as first_na3_2_0_,
        employee0_.gender as gender4_2_0_,
        employee0_.hire_date as hire_dat5_2_0_,
        employee0_.last_name as last_nam6_2_0_
    from
        employees employee0_
    where
        employee0_.emp_no=?

    update
        employees
    set
        birth_date=?,
        first_name=?,
        gender=?,
        hire_date=?,
        last_name=?
    where
        emp_no=?

    select
        employee0_.emp_no as emp_no1_2_0_,
        employee0_.birth_date as birth_da2_2_0_,
        employee0_.first_name as first_na3_2_0_,
        employee0_.gender as gender4_2_0_,
        employee0_.hire_date as hire_dat5_2_0_,
        employee0_.last_name as last_nam6_2_0_
    from
        employees employee0_
    where
        employee0_.emp_no=?

Messi
```

Queries breakdown:

1. A select performed for fetching the employee entity
2. After the flush method execution an update is performed instantly
   (if the flush method wasn't there, the update would be performed at the end of the transaction)
3. Finally, by refreshing (using refresh method) the entity, an new select is preformed and we get
   back Messi as first name
