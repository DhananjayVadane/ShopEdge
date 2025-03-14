# Step 1: Use Maven image to build the application
FROM maven:3.8.5-openjdk-17 AS build

# Set working directory
WORKDIR /app

# Copy pom.xml and dependencies separately (to leverage Docker cache)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code and build the application
COPY src ./src
RUN mvn package -DskipTests

# Step 2: Use OpenJDK to run the built application
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy the built JAR file from the Maven container
COPY --from=build /app/target/*.jar app.jar

# Expose the application port
EXPOSE 9090

# Run the Spring Boot application
CMD ["java", "-jar", "app.jar"]

