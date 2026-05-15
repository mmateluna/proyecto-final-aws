# Etapa 1: Compilación (Build)
FROM maven:3.8.5-openjdk-17-slim AS build
WORKDIR /app

# Copiamos el pom y descargamos dependencias (optimiza cache)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copiamos el código fuente y compilamos el JAR
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Ejecución (Runtime - Mínimo Privilegio)
FROM openjdk:17-jdk-slim
WORKDIR /app

# Crear usuario de sistema para no correr como root
RUN addgroup --system springuser && adduser --system springuser --ingroup springuser
USER springuser

# Copiamos solo el JAR resultante de la etapa anterior
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]