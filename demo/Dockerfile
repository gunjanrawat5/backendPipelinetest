# ---------- Build stage ----------
FROM maven:3.9-eclipse-temurin-21 AS build

WORKDIR /app

# Copy pom and download dependencies (cache layer)
COPY pom.xml .
RUN mvn -B dependency:go-offline

# Copy the source code
COPY src ./src

# Build the Spring Boot jar (skip tests for faster builds in CI)
RUN mvn -B clean package -DskipTests

# ---------- Run stage ----------
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Copy fat jar from build stage
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
