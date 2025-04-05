# Dockerfile
#FROM maven:3.8.6-eclipse-temurin-17 AS builder
#WORKDIR /app
#COPY . .
#RUN mvn clean package -DskipTests

# Étape de build
FROM maven:3.8.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml ./
RUN mvn dependency:go-offline -B
COPY src/ ./src/
RUN mvn clean package -DskipTests -Dmaven.test.skip=true

# Étape d'exécution
FROM eclipse-temurin:17-jre-jammy
RUN addgroup --system appgroup && adduser --system --no-create-home --ingroup appgroup appuser
USER appuser
COPY --from=builder --chown=appuser:appgroup /app/target/*.jar /app/app.jar
ENTRYPOINT ["java", "-Xmx512m", "-XX:+UseG1GC", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app/app.jar"]
