# Spring basics

- Controller: routes requests (@RestController)
- @RequestMapping(value = "/", methord = RequestMethod.GET || RequestMethod.POST)
- Build in local server

DevTools: integrates any changes automatically

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

1. @RequestParam
   @RequestParam(value="date", required=true/false)

2. Path variable, @RequestMapping(value = "/orders/{id}", methord = RequestMethod.GET || RequestMethod.POST)
   myMerthod(@PathVariable String id)

- Controllers pacakge must be a sub-package of the SpringBootApplication

## Spring MVC (Model View Controller)

Controller: request/response
Model: POJOs
Views: html views (passing model POJOS)

## Basic JPA

### define an entity

```java
@Entity
public class Project {

   @Id
   @GeneratedValue(strategy=GenerationType.AUTO)
   private long projectId;

}
```

```java
spring.jpa.show-sql=true // shows sql statements
```

### CRUD repository

```java
public interface ProjectRepository extends CrudRepository<Project, Long> {

}
```

### Many to one, One to many

1. one project to many employees
2. one employee one project

```java
@Entity
public class Project {

   @Id
   @GeneratedValue(strategy=GenerationType.AUTO)
   private long projectId;

   @OneToMany(mappedBy="theProject")
   private List<Employee> employees;

}
```

```java
@Entity
public class Employee {

   @Id
   @GeneratedValue(strategy=GenerationType.AUTO)
   private long employeeId;

   @ManyToOne
   @JoinColumn(name="project_id")
   private Project theProject;

}
```

--> new column to employee, project_id
--> for assigning a project to an employee you have to:

1. fetch the employees
2. for each employee set the project
3. save the employees

### Cascading

- CascadeType.ALL, propagates all operations from parent to child
- CascadeType.DETACH, When we use CascadeType.DETACH, the child entity will also get removed from the persistent context.

```java
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();

    assertThat(session.contains(person)).isTrue();
    assertThat(session.contains(address)).isTrue();

    session.detach(person);
    assertThat(session.contains(person)).isFalse();
    assertThat(session.contains(address)).isFalse();
```

- CascadeType.MERGE, propagates the merge opertaion from parent to child

```java
    Address savedAddressEntity = session.find(Address.class, addressId);
    Person savedPersonEntity = savedAddressEntity.getPerson();
    savedPersonEntity.setName("devender kumar");
    savedAddressEntity.setHouseNumber(24);
    session.merge(savedPersonEntity);
    session.flush();
```

```sh
   Hibernate: select address0_.id as id1_0_0_, address0_.city as city2_0_0_, address0_.houseNumber as houseNum3_0_0_, address0_.person_id as person_i6_0_0_, address0_.street as street4_0_0_, address0_.zipCode as zipCode5_0_0_ from Address address0_ where address0_.id=?
   Hibernate: select person0_.id as id1_1_0_, person0_.name as name2_1_0_ from Person person0_ where person0_.id=?
   Hibernate: update Address set city=?, houseNumber=?, person_id=?, street=?, zipCode=? where id=?
   Hibernate: update Person set name=? where id=?
```

- CascadeType.PERSIST, propagates the persist opertaion from parent to child
- CascadeType.REFRESH, When we use this operation with Cascade Type REFRESH, the child entity also gets reloaded from the database whenever the parent entity is refreshed.

```java
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    person.setName("Devender Kumar");
    address.setHouseNumber(24);
    session.refresh(person);

    assertThat(person.getName()).isEqualTo("devender");
    assertThat(address.getHouseNumber()).isEqualTo(23);
```

- CascadeType.REMOVE, deletes everything reltated

```java
    Person savedPersonEntity = session.find(Person.class, personId);
    session.remove(savedPersonEntity);
    session.flush();
```

```sh
   Hibernate: delete from Address where id=?
   Hibernate: delete from Person where id=?
```

### Many to many

```java
@Entity
public class Project {

   @Id
   @GeneratedValue(strategy=GenerationType.AUTO)
   private long projectId;

   @ManyToMany
   @JoinTable(name="project_employee",
              joinColumns=@JoinColumn(name="project_id"),
              inverseJoinColumn=@JoinColumn(name="employee_id"))
   private List<Employee> employees;

}
```

```java
@Entity
public class Employee {

   @Id
   @GeneratedValue(strategy=GenerationType.AUTO)
   private long employeeId;

   @ManyToMany
      @JoinTable(name="project_employee",
              joinColumns=@JoinColumn(name="employee_id"),
              inverseJoinColumn=@JoinColumn(name="project_id"))
   private List<Project> projects;

}
```

### Lazy vs Eagerly loading

(Don't use ALL and REMOVE)

- FetchType.LAZY
- FetchType.EAGER

