FROM eclipse-temurin:21 AS build

# Install required tools
RUN apt-get update && apt-get install -y git

# Clone Piped backend
RUN git clone https://github.com/TeamPiped/Piped.git /app

WORKDIR /app

# Checkout latest stable release (optional)
# RUN git checkout tags/<version>

# Disable signups by patching AuthController - error in build
# RUN sed -i 's/public User registerUser(/public User registerUserDisabled(/' \
#     src/main/java/com/github/TeamPiped/controllers/AuthController.java \
#     && echo "\n// Signups disabled automatically" >> src/main/java/com/github/TeamPiped/controllers/AuthController.java

# Build backend
RUN ./gradlew build --no-daemon

# Runtime image
FROM eclipse-temurin:21-jre
WORKDIR /app

COPY --from=build /app/build/libs/*.jar /app/piped.jar

# Environment variables for backend config
ENV PORT=8080
ENV HTTP_WORKERS=4
# ENV YOUTUBE_API_KEY= # optional if you use API key
ENV DB_DRIVER=org.h2.Driver
ENV DB_URL=jdbc:h2:file:/app/data/db

EXPOSE 8080
CMD ["java", "-jar", "piped.jar"]
