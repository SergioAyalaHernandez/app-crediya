# =========================
# Stage 1: Build
# =========================
FROM gradle:8.12-jdk17 AS build

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar archivos raíz de Gradle
COPY gradle/ gradle/
COPY gradlew gradlew
COPY gradlew.bat gradlew.bat
COPY gradle/wrapper/ gradle/wrapper/
COPY build.gradle settings.gradle main.gradle gradle.properties ./

# Copiar todos los módulos
COPY applications/ ./applications/
COPY domain/ ./domain/
COPY infrastructure/ ./infrastructure/

# Dar permisos y ejecutar build de app-service
RUN chmod +x ./gradlew && ./gradlew :app-service:build --no-daemon -x test --stacktrace

# =========================
# Stage 2: Runtime
# =========================
FROM openjdk:17-jdk-slim AS runtime

WORKDIR /app

# Copiar el JAR generado del stage de build
COPY --from=build /app/applications/app-service/build/libs/*.jar app-service.jar

# Puerto en el que correrá la app
EXPOSE 8081

# Comando para ejecutar la app
ENTRYPOINT ["java", "-jar", "app-service.jar"]
