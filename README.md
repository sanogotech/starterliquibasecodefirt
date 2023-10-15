# starterliquibasecodefirt
Starter liquidbase code first spring boot


## Top 10 des Commandes Maven Liquibase

1. **`liquibase:diff`**
   - **Description :** Génère un fichier de changelog Liquibase à partir de la différence entre la base de données existante et les entités JPA/Hibernate.
   - **Cas d'Utilisation :** Utile lorsque vous avez apporté des modifications à vos entités JPA et que vous souhaitez mettre à jour votre fichier de changelog.
   - **Exemple :** `mvn liquibase:diff`

2. **`liquibase:update`**
   - **Description :** Applique tous les changements du fichier de changelog à la base de données.
   - **Cas d'Utilisation :** Utilisé pour mettre à jour la base de données lors du déploiement d'une nouvelle version de l'application.
   - **Exemple :** `mvn liquibase:update`

3. **`liquibase:updateSQL`**
   - **Description :** Génère le SQL des changements sans les exécuter. Utile pour vérifier les scripts SQL avant leur exécution.
   - **Cas d'Utilisation :** Idéal pour examiner les modifications que Liquibase va apporter à la base de données.
   - **Exemple :** `mvn liquibase:updateSQL`

4. **`liquibase:rollback`**
   - **Description :** Annule les changements non appliqués dans la base de données.
   - **Cas d'Utilisation :** Permet de revenir en arrière en cas de problème après l'application de changements incorrects.
   - **Exemple :** `mvn liquibase:rollback -Dliquibase.rollbackCount=1`

5. **`liquibase:rollbackSQL`**
   - **Description :** Génère le SQL pour annuler les derniers changements sans les appliquer.
   - **Cas d'Utilisation :** Pour examiner les scripts SQL avant de réellement effectuer un rollback.
   - **Exemple :** `mvn liquibase:rollbackSQL -Dliquibase.rollbackCount=1`

6. **`liquibase:status`**
   - **Description :** Affiche l'état actuel de la base de données par rapport au fichier de changelog.
   - **Cas d'Utilisation :** Pour voir quels changements ont été appliqués et quels sont en attente.
   - **Exemple :** `mvn liquibase:status`

7. **`liquibase:validate`**
   - **Description :** Valide le fichier de changelog par rapport à la base de données, vérifiant l'intégrité et la syntaxe.
   - **Cas d'Utilisation :** Pour s'assurer que le fichier de changelog est correct avant de l'appliquer à la base de données.
   - **Exemple :** `mvn liquibase:validate`

8. **`liquibase:generateChangeLog`**
   - **Description :** Génère un fichier de changelog Liquibase à partir de la base de données existante.
   - **Cas d'Utilisation :** Pour initialiser un nouveau projet Liquibase en utilisant une base de données existante.
   - **Exemple :** `mvn liquibase:generateChangeLog`

9. **`liquibase:dropAll`**
   - **Description :** Supprime tous les objets de la base de données (tables, vues, etc.).
   - **Cas d'Utilisation :** Utilisé dans les scénarios de test ou pour nettoyer complètement la base de données.
   - **Exemple :** `mvn liquibase:dropAll`

10. **`liquibase:changelogSync`**
    - **Description :** Marque tous les changements dans le fichier de changelog comme appliqués sans les exécuter.
    - **Cas d'Utilisation :** Utile pour synchroniser l'état du fichier de changelog avec la base de données sans appliquer les modifications.
    - **Exemple :** `mvn liquibase:changelogSync`

N'oubliez pas de personnaliser les exemples en fonction de votre configuration et de vos besoins spécifiques dans votre projet. Ces commandes sont puissantes et doivent être utilisées avec précaution, en particulier dans les environnements de production.


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
# Configuration de la source de données H2 (base de données en mémoire)
spring.datasource.url=jdbc:h2:file:./data/sampledb
spring.datasource.username=sa
spring.datasource.password=password
spring.datasource.driver-class-name=org.h2.Driver
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
