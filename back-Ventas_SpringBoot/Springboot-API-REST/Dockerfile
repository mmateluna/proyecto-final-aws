# Stage 1: Build con Maven y Amazon Corretto (Java 17)
FROM maven:3.9.6-amazoncorretto-17 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Stage 2: Ejecución con Amazon Corretto (Más ligero y compatible con AWS)
FROM amazoncorretto:17-alpine
WORKDIR /app
# Crear un usuario de sistema para seguridad
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
# Copiamos el jar generado
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]