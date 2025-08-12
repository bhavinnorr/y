# =====================
# Stage 1 — Build
# =====================
FROM gradle:8.14.2-jdk21 AS build

# Clone repo
RUN git clone https://github.com/TeamPiped/Piped-Backend.git /app
WORKDIR /app

# Build fat JAR (shadowJar is already configured in repo)
RUN ./gradlew shadowJar --no-daemon

# =====================
# Stage 2 — Runtime
# =====================
FROM eclipse-temurin:21-jdk AS runtime

WORKDIR /app

# Copy built JAR from build stage
COPY --from=build /app/build/libs/*-all.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
