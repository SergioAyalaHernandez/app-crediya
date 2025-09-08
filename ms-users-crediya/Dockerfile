# Dockerfile para construir y ejecutar tu app
FROM gradle:8.3-jdk17 AS build

WORKDIR /app
COPY . .

# Construye la app sin tests
RUN gradle build --no-daemon -x test

# Etapa de ejecución
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
