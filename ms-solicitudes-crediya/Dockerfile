FROM gradle:8.3-jdk17 AS build
WORKDIR /app
COPY build.gradle settings.gradle ./
COPY applications/app-service/build.gradle ./
RUN gradle dependencies --no-daemon
COPY applications/app-service/src ./src
RUN gradle build --no-daemon -x test

FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
