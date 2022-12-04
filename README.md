# Quarkus :whale: Docker

## About Quarkus and this example

[Quarkus](https://quarkus.io/) is a Kubernetes Native Java stack tailored for OpenJDK HotSpot and GraalVM.

It provides a fast boot time, low RSS memory and offers near instant scale up and high density memory utilization in container orchestration platforms like Kubernetes.

In this example you can check how to dockerize a Quarkus application so you can run your container anywhere.

## Technical requirements

You are going to need only 2 things

- A Quarkus project, if you need some help at setting it up you can check how [in this example](https://github.com/codewithhades/quarkus-basic-setup)
- [Docker](https://www.docker.com), so you can build the Docker image

## How to build the docker image

- Firstly you need to package your application into a runnable JAR that will be generated withing the target folder. In order to do so remember to also specify the uber-jar packaging type within the [application.properties](src/main/resources/application.properties)
  ````properties
  quarkus.package.type=uber-jar    
  ```` 
  Then we just need to execute
  ````bash
  mvn clean package
  ````

- The next thing is to create a [Dockerfile](docker/Dockerfile) that is based on a JDK that matches the one used by your project. It should copy the previously generated JAR and run the java command as entrypoint
    ````Dockerfile
  FROM openjdk:18-jdk-slim
  COPY /target/quarkus-docker-runner.jar /app.jar
  ENTRYPOINT ["java","-jar","/app.jar"]
    ````

- With the JAR generated and your [Dockerfile](docker/Dockerfile) you can now build your image by executing
    ````bash
    docker build -t quarkus-docker -f docker/Dockerfile .
    ````
- You can make sure that the image was successfully created by listing all the images
    ````bash
    docker images
    
    REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
    quarkus-docker       latest    6fcca5332653   24 hours ago   424MB
    ````

## How to run the docker image

You can now run your image by executing the following command (making sure that you link the 8080 ports to allow http connections)
````bash
docker run -p 8080:8080 quarkus-docker
````
This should start your application within the Docker image. You can browse [localhost:8080/app/api](http://localhost:8080/app) to check the greeting message!

## And before you go...

:pray: I hope you find this example useful and if you want to support me in my mission to help our fellow Java developers please consider starring and sponsoring this space!

:coffee: May Java be with you!