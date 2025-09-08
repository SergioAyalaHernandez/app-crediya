FROM gradle:8.3-jdk17 AS build
WORKDIR /app

# Copiar los gradle scripts
COPY build.gradle ./
COPY settings.gradle ./

# Instalar dependencias
RUN gradle dependencies --no-daemon || true

# Copiar el código fuente del servicio
COPY applications/app-service/src ./src
RUN gradle build --no-daemon -x test

# Stage final
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
