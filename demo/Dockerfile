# ---------- Build stage ----------
FROM maven:3.9-eclipse-temurin-17 AS build

# Set working directory inside the container
WORKDIR /app

# Copy pom.xml and download dependencies (better caching)
COPY pom.xml .
RUN mvn -B dependency:go-offline

# Now copy the source code
COPY src ./src

# Build the Spring Boot jar (skip tests for faster builds in CI)
RUN mvn -B clean package -DskipTests

# ---------- Run stage ----------
FROM eclipse-temurin:17-jre-alpine

# Set working directory
WORKDIR /app

# Copy the fat jar from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose the port your app uses (change if needed)
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
