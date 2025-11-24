# On utilise une image JDK de base légère
FROM eclipse-temurin:23-jdk-alpine AS builder

# On définit le répertoire de travail
WORKDIR /app

# Copier les fichiers Maven pour gérer les dépendances en cache
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .

# Rendre le fichier mvnw exécutable
RUN chmod +x mvnw

# Télécharger les dépendances Maven en cache
RUN ./mvnw dependency:go-offline -B

# Copier le code source du projet
COPY src ./src

# Compiler et construire le fichier JAR
RUN ./mvnw package -DskipTests

# Étape 7 : Utiliser une image légère pour exécuter l'application
FROM eclipse-temurin:23-jre-alpine

# Étape 8 : Définir le répertoire de travail
WORKDIR /app

# Étape 9 : Copier le fichier JAR généré depuis l'étape de build
COPY --from=builder /app/target/travelo_backend-0.0.1-SNAPSHOT.jar app.jar

# Étape 10 : Exposer le port de l'application
EXPOSE 8080

# Étape 11 : Définir l'utilisateur non root pour plus de sécurité
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Étape 12 : Commande pour démarrer l'application
ENTRYPOINT ["java", "-jar", "app.jar"]
