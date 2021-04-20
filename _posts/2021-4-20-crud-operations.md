---
layout: post
title: CRUD operations using JPA/hibernate
---

A short blog how you can perform CRUD (Create Read Update Delete) operations using JPA/hibernate.
If you want to play and try your self, follow the set up instructions [here](https://konstantinosblatsoukasrepo.github.io/jpa-orm-setup/).

## CRUD (Create Read Update Delete) operations

### Set up the entity object

First we need to define a entity object (e.g. an object that is mapped to the database table).
For defying an entity you need java POJO that has the following annotations:

- At the top of the class the annotation *@Entity*
- (Optional) if the class name the is not identical to table name
  you need to add the *@Table(name="your_table_name")*, **your_table_name**
  is the actual database table name
- Each class property corresponds to a database column and is annotated with
  *@Column*
  Again, here is the property name is not the same as the tables name use
  *@Column(name="your_table_column")*, **your_table_column** is the column name
  in the database
- The property that corresponds to the primary key table is annotated with *@Id*, usually
  this is an auto-generated value, you specify that by *@Generated* annotation

For example, in the sample database the **employees** table would be mapped as follows:

```java
@Entity
@Table(name="employees")
public class Employee {

    @Id
    @GeneratedValue
    @Column(name="emp_no")
    private int employeeId;

    @Column(name="birth_date")
    private Date birthDate;

    @Column(name="first_name")
    private String firstName;

    @Column(name="last_name")
    private String lastName;

    @Column(name="gender")
    private String gender;

    @Column(name="hire_date")
    private Date hireDate;

    // getters and setters
```

### Perform basic CRUD (Create Read Update Delete) operations

#### Get the entity manager

Now that you have mapped the db table time for performing some operations!
In order to do anything with an entity you have to fetch an *EntityManager*,
an *EntityManager* can manage entities (You didn't expect that :)).

- Add to a class the following attribute:

```java
@Repository
public EmployeeRepository {

    @PersistenceContext
    EntityManager entityManager;

    // CRUD operations implementations
}

```

The *@Repository* annotation helps the spring for fetching the class instance if requested
(e.g. through the @autowire or any other injection).

All the following methods belong to **EmployeeRepository** class.

#### Create the C in CRUD

```java
    public Employee save(Employee employee) {
        entityManager.getTransaction().begin();
        entityManager.persist(employee);
        entityManager.getTransaction().commit();

        return employee;
    }

```

#### Read the R in CRUD

```java
   public Employee findById(int employeeId) {
        Employee employee = entityManager.find(Employee.class, employeeId);
        return employee;
    }
```

#### Update the U in CRUD

```java
    public Employee update(Employee employee) {
        Employee updatedEmployee = entityManager.merge(employee);
        return updatedEmployee;
    }
```

#### Delete the D in CRUD

```java
    public void deleteById(int  employeeId) {
        Employee employee = entityManager.find(Employee.class, employeeId);
        entityManager.remove(employee);
    }
```
