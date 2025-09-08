# Stage 1: Build
FROM gradle:8.4-jdk17 AS build
WORKDIR /app

ENV GRADLE_OPTS="-Dorg.gradle.jvmargs=-Xmx2048m -Dorg.gradle.daemon=false"

# Copiar archivos raíz del proyecto
COPY build.gradle settings.gradle main.gradle ./
COPY gradle/ gradle/

# Copiar todos los subproyectos necesarios
COPY applications/ ./applications/
COPY domain/ ./domain/
COPY infrastructure/ ./infrastructure/

# Ejecutar build desde el root para ms-users
RUN if [ -f "./gradlew" ]; then \
        chmod +x ./gradlew && \
        ./gradlew :applications:app-service:build --no-daemon -x test --stacktrace; \
    else \
        gradle :applications:app-service:build --no-daemon -x test --stacktrace; \
    fi

# Stage 2: Runtime
FROM openjdk:17-jdk-slim
WORKDIR /app

COPY --from=build /app/applications/app-service/build/libs/*.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
