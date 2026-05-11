# =============================================
# STAGE 1: BUILD
# =============================================
FROM maven:3.9.6-amazoncorretto-17 AS builder

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn clean package -B

# =============================================
# STAGE 2: RUNTIME
# =============================================
FROM amazoncorretto:17-al2023

WORKDIR /app

RUN groupadd -r spring && useradd -r -g spring spring

RUN mkdir -p /app/logs && chown -R spring:spring /app

USER spring

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]