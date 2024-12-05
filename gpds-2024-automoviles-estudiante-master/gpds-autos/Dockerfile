# Etapa 1: Construcción del proyecto
FROM maven:3.8.5-openjdk-17 AS build
WORKDIR /app

# Copia el archivo pom.xml para resolver dependencias
COPY pom.xml .

# Descarga las dependencias en modo offline para optimizar
RUN mvn dependency:go-offline -B

# Copia todo el código fuente al contenedor
COPY . .

# Compila y empaqueta la aplicación sin ejecutar pruebas
RUN mvn clean package -DskipTests

# Etapa 2: Imagen de producción
FROM openjdk:17-slim
WORKDIR /app

# Copia el archivo JAR generado en la etapa de construcción
COPY --from=build /app/target/gpds-autos-0.0.1-SNAPSHOT.jar ./app.jar

# Expone el puerto en el contenedor
EXPOSE 8080

# Define el comando de inicio de la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]
