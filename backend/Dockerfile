# Multi-stage build for Spring Boot backend
FROM adoptopenjdk:11-jdk-hotspot AS build

WORKDIR /app

# Copy Maven wrapper and pom.xml
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Download dependencies
RUN ./mvnw dependency:go-offline

# Copy source code
COPY src ./src

# Build the application
RUN ./mvnw clean package -DskipTests

# Runtime stage
FROM adoptopenjdk:11-jre-hotspot

WORKDIR /app

# Copy the built JAR
COPY --from=build /app/target/*.jar app.jar

# Expose port
EXPOSE 8089

# Run the application
CMD ["java", "-jar", "app.jar"]