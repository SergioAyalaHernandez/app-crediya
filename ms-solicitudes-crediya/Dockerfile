FROM gradle:8.3-jdk17 AS build
WORKDIR /app

# Limitar memoria de Gradle
ENV GRADLE_OPTS="-Dorg.gradle.jvmargs=-Xmx2048m -Dorg.gradle.daemon=false"

# Copiar proyecto
COPY applications/app-service/ ./app-service/
WORKDIR /app/app-service

# Instalar dependencias y build
RUN gradle build --no-daemon -x test --stacktrace

# Stage final
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/app-service/build/libs/*.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
