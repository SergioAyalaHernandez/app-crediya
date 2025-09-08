# Stage 1: Build
FROM gradle:8.4-jdk17 AS build

WORKDIR /app

# Copiar archivos de configuración de Gradle
COPY build.gradle settings.gradle main.gradle ./
COPY gradle/ gradle/
COPY gradlew gradlew
COPY gradlew.bat gradlew.bat
COPY gradle/wrapper/ gradle/wrapper/
COPY gradle.properties ./

# Copiar código fuente
COPY applications/ ./applications/
COPY domain/ ./domain/
COPY infrastructure/ ./infrastructure/

# Dar permisos y ejecutar build
RUN chmod +x ./gradlew && ./gradlew :app-service:build --no-daemon -x test --stacktrace

# Stage 2: Runtime
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copiar jar desde build stage
COPY --from=build /app/app-service/build/libs/*.jar app-service.jar

# Comando para ejecutar la app
ENTRYPOINT ["java","-jar","app-service.jar"]
