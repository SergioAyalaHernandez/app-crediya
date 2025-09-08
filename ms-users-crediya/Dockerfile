# Stage 1: Build
FROM gradle:8.4-jdk17 AS build

WORKDIR /app

# Copiar archivos de configuración de Gradle
COPY build.gradle settings.gradle main.gradle ./
COPY gradle/ gradle/
COPY gradle.properties ./

# Copiar todo el código fuente
COPY applications/ ./applications/
COPY domain/ ./domain/
COPY infrastructure/ ./infrastructure/

# Ejecutar build del módulo app-service
RUN chmod +x ./gradlew && ./gradlew :app-service:build --no-daemon -x test --stacktrace

# Stage 2: Runtime
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copiar jar generado desde la etapa de build
COPY --from=build /app/app-service/build/libs/*.jar app-service.jar

# Comando para ejecutar la app
ENTRYPOINT ["java","-jar","app-service.jar"]
