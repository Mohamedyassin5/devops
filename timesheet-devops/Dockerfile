# Utiliser Java 17 (compatible Spring Boot 3.x)
FROM eclipse-temurin:17-jdk-alpine

# Copier le JAR compilé
ADD target/timesheet-devops-1.0.jar timesheet-devops-1.0.jar

# Point d'entrée
ENTRYPOINT ["java", "-jar", "/timesheet-devops-1.0.jar"]
