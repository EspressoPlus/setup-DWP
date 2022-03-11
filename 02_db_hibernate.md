# make database in MySQL Workbench
* demo this
* database is hbstudent
* 

# Hibernate ORM
* ORM = Object-to-Relational Mapping
* framework for persisting Java objects into a database
* it handles low-level SQL-related code .. minimizes need for JDBC code
* makes it easier to retrieve/store objects to database by mapping classes to db table
* can use config files or annotations to set up mappings

# preliminary example: saving Java Object with Hibernate
* make an object:

```
Student tempStudent = new Student("Paul", "Wall", "paul@shoe.com");
```

* save it to the db

```
System.out.println("saving the student to the db ...");
session.save(tempStudent);
```

# project setup: pom.xml
* https://mvnrepository.com/
* get "hibernate-core" and "mysql-connector-java" and add to your maven project 

# test database connection
* testJDBC.java

# create Hibernate Configuration file: hibernate.cfg.xml (old school)
* tells Hibernate how to connect to database
* includes URL, user, password
* can put this in root of src dir, or use project's classpath

# use Java/Hibernate Annotations to map java to database
### map class to database table
```
@Entity
@Table(name="student") // optional to explicitly name bc table and entity names are same
public class Student {
...
}
```
### map Java fields to database columns
```
@Id
@Column(name="id")
private int id;

@Column(name="first_name") // mapping Java firstName to db "first_name"
private String firstName;
...
}
```
### write some code to annotate the Java class
* make Student.java class
* videos say use javax, I used jakarta .. not sure why
```
package edu.wccnet.ckingdon.hibernate.demo.entity;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;


@Entity
@Table(name="student")
public class Student {
	
	@Id // this tags it as the primary key field
	@GeneratedValue(strategy = GenerationType.IDENTITY) // use the IDENTITY field (i.e. primary key) to auto-generate value
	@Column(name="id") // specify the field's name in the actual database here
	private int id;
	// in 14-Primary_Keys_part_1.webm @ 04:00 
	// what about custom IDs ??
	// can create a custom generator
	// 1) create subclass of org.hibernate.id.SequenceGenerator
	// 2) override the method: public Serializable generate (...) include custom business loginc in new generate()
	
	
	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
		
	// empty contructor
	public Student() {}


	public Student(String firstName, String lastName, String email) {
		this.firstName = firstName;
		this.lastName = lastName;
		this.email = email;
	}


	public int getId() {
		return id;
	}


	public void setId(int id) {
		this.id = id;
	}


	public String getFirstName() {
		return firstName;
	}


	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}


	public String getLastName() {
		return lastName;
	}


	public void setLastName(String lastName) {
		this.lastName = lastName;
	}


	public String getEmail() {
		return email;
	}


	public void setEmail(String email) {
		this.email = email;
	}


	@Override
	public String toString() {
		return "Student [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + ", email=" + email + "]";
	}
	
	
	
}

```





