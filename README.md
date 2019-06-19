Spring Boot Crud Operations Example

1. Introduction:-
  - Spring Boot is a module that provides rapid application development feature to the spring framework including auto-configuration, standalone-code, and production-ready code.
  
  - It creates applications that are packaged as jar and are directly started using embedded servlet container (such as Tomcat, Jetty or Undertow). Thus, no need to deploy the war files.
  
  - It simplifies the maven configuration by providing the starter template and helps to resolve the dependency conflicts. It automatically identifies the required dependencies and imports them in the application.
  
  - It helps in removing the boilerplate code, extra annotations, and xml configurations.
  
  - It provides a powerful batch processing and manages the rest endpoints.
  
  - It provides an efficient jpa-starter library to effectively connect the application with the relational databases.
  
  Now, open the eclipse ide and let us see how to implement this tutorial in the spring boot module using the jpa-starter library to communicate with a relational database.

2. Spring Boot Crud Operations Example
    Here is a systematic guide for implementing this tutorial.

    2.1 Tools Used
      We are using STS, JDK 8, and Maven.
      
3. Creating a Spring Boot application
    Below are the steps involved in developing the application.
     
    3.1 Maven Dependencies
       Here, we specify the dependencies for the Spring Boot, Spring Boot JPA, and MySQL connector. Maven will automatically resolve the other dependencies. The updated file will have the following code.
       
    pom.xml refer this file.
        
    3.2 Application Properties
      Create a new properties file at the location: Springbootcrudoperation/src/main/resources/ and add the following code to it.

        application.properties
        Refere the application.properties and change the properties as per your configuration.
        ## Spring datasource.
        spring.datasource.driver.class=com.mysql.cj.jdbc.Driver
        spring.datasource.url=jdbc:mysql://localhost:3306/paramount
        spring.datasource.username=root
        spring.datasource.password=
 
        ## Hibernate properties.
        spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
 
        ## Show sql query.
        spring.jpa.show-sql=true
 
        ## Hibernate ddl auto.
        
      3.3 Java Classes
        Let us write all the java classes involved in this application.

      3.3.1 Implementation/Main class
        Add the following code the main class to bootstrap the application from the main method. Always remember, the entry point of the spring boot application is the class containing @SpringBootApplication annotation and the static main method.

        Myapplication.java
        package com.ducat.springboot.rest;
 
        import org.springframework.boot.SpringApplication;
        import org.springframework.boot.autoconfigure.SpringBootApplication;
 
        @SpringBootApplication
        public class Myapplication {
 
        public static void main(String[] args) {
          SpringApplication.run(Myapplication.class, args);
         }
        }
        
      3.3.2 Model class
      Add the following code to the employee model class.

        Employee.java
        package com.ducat.springboot.rest.model;
 
        import javax.persistence.Entity;
        import javax.persistence.GeneratedValue;
        import javax.persistence.GenerationType;
        import javax.persistence.Id;
        import javax.persistence.Table;

        import org.hibernate.annotations.DynamicInsert;
        import org.hibernate.annotations.DynamicUpdate;
        import org.springframework.stereotype.Component;

        @Component

        // Spring jpa jars.
        @Entity
        @Table(name= "employee")
 
        // To increase speed and save sql statement execution time.
        @DynamicInsert
        @DynamicUpdate
        public class Employee { 
        @Id
        @GeneratedValue(strategy= GenerationType.IDENTITY)
        private int id;
        private String name;
        private String department;
        private double salary;

        public Employee() { }

        public int getId() {
            return id;
        }
        public void setId(int id) {
            this.id = id;
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public String getDepartment() {
            return department;
        }
        public void setDepartment(String department) {
            this.department = department;
        }
        public double getSalary() {
            return salary;
        }
        public void setSalary(double salary) {
            this.salary = salary;
        }

        @Override
        public String toString() {
            return "Employee [id=" + id + ", name=" + name + ", department=" + department + ", salary=" + salary + "]";
        }
        }
    
      3.3.3 Data-Access-Object interface
      Add the following code to the Dao interface that extends the JPA repository to automatically handle the crud queries.

    Mydaorepository.java
    
        package com.ducat.springboot.rest.dao;

        import org.springframework.data.jpa.repository.JpaRepository;
        import org.springframework.stereotype.Repository;

        import com.ducat.springboot.rest.model.Employee;

        @Repository
        public interface Mydaorepository extends JpaRepository<Employee, Integer> {

        }
        
     3.3.4 Service class
      Add the following code to the service class where we will call the methods of the Dao interface to handle the sql operations.

      Mydaorepository.java
      
        package com.ducat.springboot.rest.service;

        import java.util.List;
        import java.util.Optional;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Service;

        import com.ducat.springboot.rest.dao.Mydaorepository;
        import com.ducat.springboot.rest.model.Employee;

        @Service
        public class Myserviceimpl implements Myservice {
 
        @Autowired
        Mydaorepository dao;

        @Override
        public List<Employee> getEmployees() {
            return dao.findAll();
        }
        @Override
        public Optional<Employee> getEmployeeById(int empid) {
            return dao.findById(empid);
        }
        @Override
        public Employee addNewEmployee(Employee emp) {
            return dao.save(emp);
        }
        @Override
        public Employee updateEmployee(Employee emp) {
            return dao.save(emp);
        }
        @Override
        public void deleteEmployeeById(int empid) {
            dao.deleteById(empid);
        }
        @Override
        public void deleteAllEmployees() {
            dao.deleteAll();
        }
      }
    
   3.3.5 Controller class
   Add the following code to the controller class designed to handle the incoming requests. The class is annotated with the @RestController annotation where every method returns a domain object as a json response instead of a view.

   Mycontroller.java
   
              package com.ducat.springboot.rest.controller;

        import java.util.List;
        import java.util.Optional;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.web.bind.annotation.PathVariable;
        import org.springframework.web.bind.annotation.RequestBody;
        import org.springframework.web.bind.annotation.RequestMapping;
        import org.springframework.web.bind.annotation.RequestMethod;
        import org.springframework.web.bind.annotation.RestController;

        import com.ducat.springboot.rest.model.Employee;
        import com.ducat.springboot.rest.service.Myservice;

        @RestController
        public class Mycontroller {

        @Autowired
        Myservice service;

        @RequestMapping(value= "/employee/all", method= RequestMethod.GET)
        public List<Employee> getEmployees() {
            System.out.println(this.getClass().getSimpleName() + " - Get all employees service is invoked.");
            return service.getEmployees();
        }

        @RequestMapping(value= "/employee/{id}", method= RequestMethod.GET)
        public Employee getEmployeeById(@PathVariable int id) throws Exception {
            System.out.println(this.getClass().getSimpleName() + " - Get employee details by id is invoked.");
 
        Optional<Employee> emp =  service.getEmployeeById(id);
        if(!emp.isPresent())
            throw new Exception("Could not find employee with id- " + id);
 
        return emp.get();
    }
 
        @RequestMapping(value= "/employee/add", method= RequestMethod.POST)
        public Employee createEmployee(@RequestBody Employee newemp) {
            System.out.println(this.getClass().getSimpleName() + " - Create new employee method is invoked.");
            return service.addNewEmployee(newemp);
        }

        @RequestMapping(value= "/employee/update/{id}", method= RequestMethod.PUT)
        public Employee updateEmployee(@RequestBody Employee updemp, @PathVariable int id) throws Exception {
            System.out.println(this.getClass().getSimpleName() + " - Update employee details by id is invoked.");
 
        Optional<Employee> emp =  service.getEmployeeById(id);
        if (!emp.isPresent())
            throw new Exception("Could not find employee with id- " + id);
 
        /* IMPORTANT - To prevent the overriding of the existing value of the variables in the database, 
         * if that variable is not coming in the @RequestBody annotation object. */    
        if(updemp.getName() == null || updemp.getName().isEmpty())
            updemp.setName(emp.get().getName());
        if(updemp.getDepartment() == null || updemp.getDepartment().isEmpty())
            updemp.setDepartment(emp.get().getDepartment());
        if(updemp.getSalary() == 0)
            updemp.setSalary(emp.get().getSalary());
 
        // Required for the "where" clause in the sql query template.
        updemp.setId(id);
        return service.updateEmployee(updemp);
        }
 
        @RequestMapping(value= "/employee/delete/{id}", method= RequestMethod.DELETE)
        public void deleteEmployeeById(@PathVariable int id) throws Exception {
            System.out.println(this.getClass().getSimpleName() + " - Delete employee by id is invoked.");
 
        Optional<Employee> emp =  service.getEmployeeById(id);
        if(!emp.isPresent())
            throw new Exception("Could not find employee with id- " + id);
 
        service.deleteEmployeeById(id);
        }

        @RequestMapping(value= "/employee/deleteall", method= RequestMethod.DELETE)
        public void deleteAll() {
            System.out.println(this.getClass().getSimpleName() + " - Delete all employees is invoked.");
            service.deleteAllEmployees();
        }
        }


4. Run the Application
As we are ready with all the changes, let us compile the spring boot project and run the application as a java project. Right click on the Myapplication.java class, Run As -> Java Application.

Developers can debug the example and see what happens after every step. Enjoy!

5. Project Demo
Open the postman tool and hit the following urls to display the data in the json format.

        // Get all employees
        http://localhost:8080/employee/all

        // Get employee by id
        http://localhost:8080/employee/1003

        // Create new employee
        http://localhost:8080/employee/add

        // Update existing employee by id
        http://localhost:8080/employee/update/1008

        // Delete employee by id
        http://localhost:8080/employee/delete/1002

        // Delete all employees
        http://localhost:8080/employee/deleteall


That is all for this tutorial and I hope the article served you whatever you were looking for. Happy Learning and do not forget to share!

6. Conclusion
In this section, developers learned how to create a spring boot application and perform the basic crud operations using the spring-jpa-starter library.

6.1 Important points
We instructed the hibernate to connect to a mysql database and use the MySQL5Dialect for generating the optimized sql queries
spring.jpa.hibernate.ddl-auto=validate will instruct the hibernate to validate the table schema at the application startup
spring.jpa.show-sql=true will instruct the hibernate framework to log all the SQL statements on the console
Developers can change the data-source details as per their need
Few more. . . . . . .
Developers can download the sample application as an Eclipse/STS project in the Downloads section.

7. Download the Eclipse/STS Project
This was an example of performing the crud operations using the spring boot module of the spring framework.
