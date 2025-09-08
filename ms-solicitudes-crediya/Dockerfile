# Stage 1: Build
FROM gradle:8.4-jdk17 AS build
WORKDIR /app

ENV GRADLE_OPTS="-Dorg.gradle.jvmargs=-Xmx2048m -Dorg.gradle.daemon=false"

# Copiar archivos raíz
COPY build.gradle settings.gradle main.gradle ./

# Copiar todos los subproyectos
COPY applications/ ./applications/
COPY sqs-sender/ ./sqs-sender/
COPY metrics/ ./metrics/
COPY rest-consumer/ ./rest-consumer/
COPY r2dbc-postgresql/ ./r2dbc-postgresql/
COPY reactive-web/ ./reactive-web/
COPY model/ ./model/
COPY usecase/ ./usecase/

# Ejecutar build desde el root para app-service
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
