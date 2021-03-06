# Kogito with Validation API

## Description

A quickstart project that shows the use of business rules and processes with data annotated with Validation API to ensure correctness

This example shows

* make use of DRL to define rules
* make use of business rules task in the process to evaluate rules
* data model annotated with Validation API
	
	
<p align="center"><img width=75% height=50% src="docs/images/process.png"></p>


A data model is easily configured with constraints to be enforced on runtime

```
import javax.validation.constraints.Email;
import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

public class Person {

    @NotNull
    @Size(min = 2, max = 30)
    private String name;

    @NotNull
    @Min(value = 2, message = "Age must be higher than 5")
    private int age;

    @NotNull
    @Email
    private String email;

    private boolean adult;
```
## Build and run

### Prerequisites
 
You will need:
  - Java 1.8.0+ installed 
  - Environment variable JAVA_HOME set accordingly
  - Maven 3.5.4+ installed

### Compile and Run in Local Dev Mode

```
mvn clean package spring-boot:run    
```


### Compile and Run using uberjar

```
mvn clean package 
```
  
To run the generated native executable, generated in `target/`, execute

```
java -jar target/kogito-business-rules-sprintboot-{version}.jar
```

### Use the application


### Submit a valid request

To make use of this application it is as simple as putting a sending request to `http://localhost:8080/persons`  with following content 

```
{
  "person" : {
    "name" "john",
    "age" : 20,
    "email" : "user@email.com"
  }
}

```

Complete curl command can be found below:

```
curl -X POST -H 'Content-Type:application/json' -H 'Accept:application/json' -d '{"person" : {"name" : "john", "age" : 20, "email" : "user@email.com"}}' http://localhost:8080/persons
```

### Submit a invalid request

To make use of this application it is as simple as putting a sending request to `http://localhost:8080/persons`  with following content 

```
{
  "person" : {
    "name" "john",
    "age" : 20,
    "email" : "user"
  }
}

```

Complete curl command can be found below:

```
curl -X POST -H 'Content-Type:application/json' -H 'Accept:application/json' -d '{"person" : {"name" : "john", "age" : 20, "email" : "user"}}' http://localhost:8080/persons
```

Invalid request will return 400 Bad Request response with information about violated constraints.

```
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8080 (#0)
> POST /persons HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.54.0
> Content-Type:application/json
> Accept:application/json
> Content-Length: 59
>
* upload completely sent off: 59 out of 59 bytes
< HTTP/1.1 400 Bad Request
< Content-Length: 262
< validation-exception: true
< Content-Type: application/json
<
* Connection #0 to host localhost left intact
{"exception":null,"propertyViolations":[],"classViolations":[],"parameterViolations":[{"constraintType":"PARAMETER","path":"createResource_persons.resource.person.email","message":"must be a well-formed email address","value":"user"}],"returnValueViolations":[]}