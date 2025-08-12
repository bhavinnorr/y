# =====================
# Stage 1 — Build
# =====================
FROM gradle:8.14.2-jdk21 AS build

# Install git
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Clone the repo
RUN git clone https://github.com/TeamPiped/Piped-Backend.git /app
WORKDIR /app

# Add Shadow plugin to build.gradle (for fat JAR)
RUN sed -i '/plugins {/a\    id "com.github.johnrengelman.shadow" version "8.1.1"' build.gradle && \
    echo '\nshadowJar {\n    archiveBaseName.set("app")\n    archiveClassifier.set("")\n    archiveVersion.set("")\n}' >> build.gradle

# Build fat jar
RUN ./gradlew shadowJar --no-daemon

# =====================
# Stage 2 — Runtime
# =====================
FROM eclipse-temurin:21-jdk AS runtime

WORKDIR /app

# Copy the fat JAR
COPY --from=build /app/build/libs/app.jar app.jar

# Expose port
EXPOSE 8080

# Run the JAR
CMD ["java", "-jar", "app.jar"]
