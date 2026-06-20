# ETAPA 1: Compilación (Build)
FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app

# Copiamos todo el contenido de la carpeta actual al contenedor
COPY . .

# Damos permiso de ejecución al wrapper de maven por si acaso
RUN chmod +x mvnw

# Compilamos el proyecto (esto genera el archivo .jar)
RUN ./mvnw clean package -DskipTests

# ETAPA 2: Ejecución (Run)
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Seguridad: Usuario no root para cumplir con la rúbrica
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Copiamos el JAR generado en la etapa anterior
COPY --from=build /app/target/*.jar app.jar

ENTRYPOINT ["java", "-jar", "app.jar"]