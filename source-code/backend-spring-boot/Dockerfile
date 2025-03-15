# Use the official Maven image with OpenJDK 17 for the build phase
FROM maven:3.8.4-openjdk-17 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the pom.xml and any other necessary files for building the project
COPY pom.xml .

# Download dependencies (this will cache dependencies layer)
RUN mvn dependency:go-offline

# Copy the rest of the application source code
COPY src ./src

# Build the JAR file
RUN mvn clean package -DskipTests

# Use a different OpenJDK 17 runtime image for the final image
FROM openjdk:17 AS runtime

# Set the working directory inside the container
WORKDIR /app

# Copy the generated JAR file from the build phase
COPY --from=build /app/target/*.jar app.jar

# Expose port 9090 (or your configured port)
EXPOSE 7070

# Run the Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
