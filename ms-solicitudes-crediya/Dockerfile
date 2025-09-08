# Stage 1: Build
FROM gradle:8.3-jdk17 AS build
WORKDIR /app

ENV GRADLE_OPTS="-Dorg.gradle.jvmargs=-Xmx2048m -Dorg.gradle.daemon=false"


COPY build.gradle settings.gradle ./
COPY applications/ ./applications/
COPY main.gradle ./

RUN if [ -f "./gradlew" ]; then \
        chmod +x ./gradlew && \
        ./gradlew :applications:app-service:build --no-daemon -x test --stacktrace; \
    else \
        gradle :applications:app-service:build --no-daemon -x test --stacktrace; \
    fi

# Stage 2: Runtime
FROM openjdk:17-jdk-slim
WORKDIR /app

# Copiar jar construido
COPY --from=build /app/applications/app-service/build/libs/*.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
