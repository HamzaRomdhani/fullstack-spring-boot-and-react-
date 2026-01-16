#FROM openjdk:11
#VOLUME /tmp
#ARG JAR_FILE=target/*.jar
#COPY ${JAR_FILE} app.jar
#ENTRYPOINT ["java","-jar","/app.jar"]


# spring-backend/Dockerfile (multi-stage)
# Build stage with Maven and JDK 11
FROM eclipse-temurin:11-jdk AS build
WORKDIR /workspace
# copy maven wrapper and settings if present
COPY .mvn .mvn
COPY mvnw mvnw
COPY pom.xml .
# download dependencies first
RUN chmod +x mvnw || true
RUN ./mvnw -B -ntp dependency:go-offline || mvn -B -ntp dependency:go-offline

# copy sources and build
COPY src src
RUN ./mvnw -B -DskipTests package

# Runtime stage
FROM eclipse-temurin:11-jre
WORKDIR /app
ARG JAR_FILE=/workspace/target/*.jar
COPY --from=build ${JAR_FILE} app.jar
ENV JAVA_OPTS="-Xms256m -Xmx512m"
EXPOSE 8080
ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar /app/app.jar"]
