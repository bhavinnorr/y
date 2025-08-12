# Stage 1 — Build
FROM gradle:8.4-jdk17 AS build

RUN git clone https://github.com/TeamPiped/Piped-Backend.git /app
WORKDIR /app

RUN ./gradlew build --no-daemon

# Stage 2 — Runtime
FROM eclipse-temurin:17-jdk

WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar

EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
