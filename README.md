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
       
      pom.xml
      
        Refer pom.xml file.
        
      3.2 Application Properties
        Create a new properties file at the location: Springbootcrudoperation/src/main/resources/ and add the following code to it.

        application.properties
        Refere the application.properties and change the properties as per your configuration.
