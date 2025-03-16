# Build Stage
FROM maven:3.8.4-openjdk-17 AS build
WORKDIR /app

# Copy pom.xml and resolve dependencies (cache layer)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code and build the application
COPY src ./src
RUN mvn clean package -DskipTests -T 1C  # Enables multithreading

# Production Runtime
FROM eclipse-temurin:17-jdk-jammy AS runtime  # Smaller base image
WORKDIR /app

# Copy only the built JAR from the build stage
COPY --from=build /app/target/*.jar app.jar

# Use a non-root user for security (optional but recommended)
RUN useradd -m myuser
USER myuser

# Expose the application port
EXPOSE 7070

# Run the Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
