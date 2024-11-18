# Start with a Maven image that includes Java 11
FROM maven:3.8.6-openjdk-11 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the pom.xml and any other necessary files to install dependencies
COPY pom.xml .

# Download dependencies and cache them
# Workaround - certs unavailable in container cause failure so temporarily disable SSL verification in the Docker build
RUN mvn -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true dependency:go-offline -B

# Copy the rest of the project files
COPY . .

# Build and test the project
RUN mvn clean install -DskipTests=false

# Use a lightweight JRE image to run the application (optional)
FROM openjdk:11-jre-slim AS runtime

# Copy the built application from the build stage
COPY --from=build /app/target/*.jar /app/app.jar

# Expose the port your application listens on
EXPOSE 8080

# Set the entrypoint to run the jar file
# In Spring Boot, profiles (like dev, test, prod, and custom profiles like docker) allow you to configure settings based on the environment.
# For example, the docker profile might point to a different database, use different credentials, or adjust other environment-specific settings.
ENTRYPOINT ["java", "-jar", "/app/app.jar", "--spring.profiles.active=docker"]
