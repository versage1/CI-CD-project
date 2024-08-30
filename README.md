# Spring Boot based Java web application
 
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml 
at the root directory of the repository.

This is a MVC architecture based application where controller returns a page with title and message attributes to the view.

## Execute the application locally and access it using your browser


 ![image](https://github.com/user-attachments/assets/aa234bf4-e641-4412-ac82-a9330367eed6)


```git clone https://github.com/Angecalais97/CI-CD-project
```

Execute the Maven targets to generate the artifacts

```mvn clean package
```

The above maven target stroes the artifacts to the `target` directory. You can either execute the artifact on your local machine
(or) run it as a Docker container.

** Note: To avoid issues with local setup, Java versions and other dependencies, I would recommend the docker way. **


### Execute locally (Java 11 needed) and access the application on http://localhost:8080

```
java -jar target/spring-boot-web.jar
```

### The Docker way

Build the Docker Image

```
docker build -t image-name .
```

```
docker run -d -p 8010:8080 -t image-name
```

Hurray !! Access the application on `http://<ip-address>:8010`


## Next Steps

### Configure a Sonar Server locally

```
Hurray !! Now you can access the `SonarQube Server` on `http://<ip-address>:9000` 


