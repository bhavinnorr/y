############################################
# 1. Build Stage
############################################
FROM gradle:8.14.2-jdk21 AS build

# Install Git
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Clone Piped backend
RUN git clone https://github.com/TeamPiped/Piped-Backend.git /app
WORKDIR /app

# Update submodule (NewPipeExtractor) to avoid YouTube parsing errors
RUN git submodule update --init --recursive && \
    cd NewPipeExtractor && \
    git fetch origin && git checkout master && git pull origin master

# Build fat JAR with all dependencies
RUN ./gradlew shadowJar --no-daemon

############################################
# 2. Runtime Stage
############################################
FROM eclipse-temurin:21-jre

WORKDIR /app

# Copy built fat JAR
COPY --from=build /app/build/libs/*-all.jar piped.jar

# Add configuration properties (disables Matrix, uses defaults)
# COPY config.properties /app/config.properties

# Optional: Add entrypoint / healthcheck scripts if needed
# COPY hotspot-entrypoint.sh docker-healthcheck.sh /app/
# HEALTHCHECK and ENTRYPOINT can be added similarly if used upstream.

EXPOSE 8080

# Launch
CMD ["java", "-jar", "piped.jar"]
