FROM gradle:8.3-jdk17 AS build
WORKDIR /app

# Copiar proyecto
COPY applications/app-service/ ./app-service/
WORKDIR /app/app-service

# Limitar memoria de Gradle
ENV GRADLE_OPTS="-Dorg.gradle.jvmargs=-Xmx1024m"

# Usar Gradle Wrapper si existe y refrescar dependencias para que descargue plugins
RUN if [ -f "./gradlew" ]; then \
        ./gradlew build --no-daemon -x test --refresh-dependencies --stacktrace; \
    else \
        gradle build --no-daemon -x test --refresh-dependencies --stacktrace; \
    fi

# Stage final
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/app-service/build/libs/*.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
