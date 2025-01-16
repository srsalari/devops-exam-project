# 1. Use an official OpenJDK (or any other Java) runtime as a parent image.
FROM openjdk:17-jdk-slim

# 2. Set the working directory inside the container.
WORKDIR /app

# 3. Copy the built JAR file from your local machine's "target" folder to the container.
#    IMPORTANT: Replace 'my-app-1.0-SNAPSHOT.jar' with your actual JAR filename.
COPY target/my-app-1.0-SNAPSHOT.jar /app/my-app.jar

# 4. Expose the port that your application will run on (optional, but a common practice).
#    If your app doesn’t bind to 8080, adjust accordingly.
EXPOSE 8080

# 5. Run the JAR file.
CMD ["java", "-jar", "my-app.jar"]
