# Dockerfile
#FROM maven:3.8.6-eclipse-temurin-17 AS builder
#WORKDIR /app
#COPY . .
#RUN mvn clean package -DskipTests

#FROM openjdk:17-alpine
#COPY --from=builder /app/target/*.jar /app/app.jar
#ENTRYPOINT ["java", "-jar", "/app/app.jar"]
# Étape de build
FROM maven:3.8.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml ./
RUN mvn dependency:go-offline
COPY src/ ./src/
RUN mvn clean package -DskipTests

# Étape d'exécution
FROM eclipse-temurin:17-jre-jammy
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
COPY --from=builder --chown=appuser:appgroup /app/target/spring-boot-app-*.jar /app/app.jar
ENTRYPOINT ["java", "-Xmx512m", "-XX:+UseG1GC", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app/app.jar"]
