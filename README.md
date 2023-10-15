# starterliquibasecodefirt
Starter liquidbase code first spring boot



## Résumé de la démarche Code First Liquibase

Voici comment vous procédez :

1. **Modifiez votre Entité Java :** Apportez les modifications nécessaires à votre entité Java, par exemple, ajoutez de nouveaux champs.

2. **Générez un Nouveau Fichier de Changelog avec Maven :**

   ```bash
   mvn liquibase:diff
    ```
Cela générera un fichier de changelog Liquibase (002-additional-fields.xml dans l'exemple précédent) basé sur les modifications dans vos entités Java.

Exécutez la Mise à Jour avec Maven :

 ```bash

mvn liquibase:update
 ```
 
Cela exécutera les changements décrits dans le fichier de changelog et mettra à jour la base de données en conséquence.

Ainsi, avec spring.jpa.hibernate.ddl-auto=none, les tables de votre base de données ne changeront pas automatiquement en fonction des modifications de votre code d'entité Java. Vous devez utiliser Liquibase pour générer et appliquer manuellement les changements de schéma. Cela offre un contrôle plus précis et permet de versionner les modifications de la base de données.

Étapes pour l'Approche "Code First" avec Liquibase :
1. Configuration dans application.properties :
properties

  ```properties
# Configuration de la source de données
spring.datasource.url=jdbc:postgresql://localhost:5432/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.hibernate.ddl-auto=none

# Configuration de Liquibase
liquibase.change-log=classpath:db/changelog/db.changelog-master.xml
liquibase.contexts=development
liquibase.shouldRun=true
liquibase.default-schema=public
 ```
 
2. Entité Java Avant Modification (2 Attributs) :

 ```java

@Entity
@Table(name = "votre_table")
public class VotreEntite {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "nom")
    private String nom;

    // Getters et Setters
}
 ```
 
3. Générer le Fichier de Changement Initial :

 ```bash

mvn liquibase:generateChangeLog
 ```
 
Cette commande générera un fichier de changelog Liquibase initial (001-initial-schema.xml dans src/main/resources/db/changelog/changes) basé sur votre schéma de base de données existant.

4. Fichier de Changelog Master (db.changelog-master.xml) Avant Modification :

 ```xml

<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.4.xsd">

    <!-- Fichier de changelog initial -->
    <include file="classpath:/db/changelog/changes/001-initial-schema.xml"/>

</databaseChangeLog>
 ```
 
5. Entité Java Après Modification (5 Attributs) :

 ```java

@Entity
@Table(name = "votre_table")
public class VotreEntite {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "nom")
    private String nom;

    @Column(name = "age")
    private int age;

    @Column(name = "email")
    private String email;

    @Column(name = "adresse")
    private String adresse;

    @Column(name = "telephone")
    private String telephone;

    // Getters et Setters
}
 ```
 
6. Générer le Nouveau Fichier de Changement :

 ```bash

mvn liquibase:diff
 ```
 
Cette commande générera un nouveau fichier de changelog Liquibase (002-additional-fields.xml dans src/main/resources/db/changelog/changes) basé sur les modifications apportées à votre entité Java.

7. Mettre à Jour la Base de Données avec les Nouvelles Modifications :

 ```bash

mvn liquibase:update
 ```
 
Cette commande exécutera les mises à jour de Liquibase en utilisant les fichiers de changelog générés, mettant à jour votre schéma de base de données en fonction des modifications de votre entité Java.

En suivant ces étapes, vous pouvez gérer efficacement votre schéma de base de données en utilisant l'approche "code first" avec Liquibase dans un projet Spring Boot.
