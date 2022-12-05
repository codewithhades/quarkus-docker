FROM openjdk:19-jdk-slim
COPY /target/quarkus-docker-runner.jar /app.jar
ENTRYPOINT ["java","-jar","/app.jar"]