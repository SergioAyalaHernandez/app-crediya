# Stage 1: Build
FROM gradle:8.4-jdk17 AS build
WORKDIR /app

ENV GRADLE_OPTS="-Dorg.gradle.jvmargs=-Xmx2048m -Dorg.gradle.daemon=false"

# Copiar archivos raíz
COPY build.gradle settings.gradle main.gradle ./

# Copiar todos los subproyectos necesarios
COPY applications/ ./applications/
COPY reactive-web/ ./reactive-web/
COPY r2dbc-postgresql/ ./r2dbc-postgresql/
COPY model/ ./model/
COPY usecase/ ./usecase/

# Ejecutar build desde el root para ms-users
RUN if [ -f "./gradlew" ]; then \
        chmod +x ./gradlew && \
        ./gradlew :applications:ms-users:build --no-daemon -x test --stacktrace; \
    else \
        gradle :applications:ms-users:build --no-daemon -x test --stacktrace; \
    fi

# Stage 2: Runtime
FROM openjdk:17-jdk-slim
WORKDIR /app

COPY --from=build /app/applications/ms-users/build/libs/*.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
