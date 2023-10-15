# Ajouter des entités/tables à un projet existant basé sur JHipster

* Source: http://chinomsoikwuagwu.com/2020/09/03/Add-tables-to-existing-jhipster-based-project/

## Qu'est-ce que JHipster ?

JHipster est un générateur de code d'application full-stack. En d'autres termes, utilisez JHipster pour générer du code d'application pour une backend Spring Boot (avec Java ou Kotlin), Micronaut, Quarkus, Node.js ou .NET, ainsi que pour un frontend Angular, React ou Vue.

Une partie du processus de génération de code d'application consiste à générer la logique de domaine, y compris les classes d'entités. La méthode recommandée est d'utiliser un fichier .jdl. JDL signifie JHipster Domain Language. C'est un langage de domaine spécifique à JHipster où vous pouvez décrire toutes vos applications, déploiements, entités et leurs relations dans un fichier unique (ou plusieurs) avec une syntaxe conviviale.

Voici un exemple de fichier .jdl pour un blog :

```jdl

application {
  config {
    baseName blog,
    applicationType monolith,
    packageName com.jhipster.demo.blog,
    prodDatabaseType mysql,
    cacheProvider hazelcast,
    buildTool maven,
    clientFramework react,
    useSass true,
    testFrameworks [protractor]
  }
  entities *
}

entity Blog {
  name String required minlength(3)
  handle String required minlength(2)
}

entity Post {
  title String required
  content TextBlob required
  date Instant required
}

entity Tag {
  name String required minlength(2)
}

relationship ManyToOne {
  Blog{user(login)} to User
  Post{blog(name)} to Blog
}

relationship ManyToMany {
  Post{tag(name)} to Tag{entry}
}

paginate Post, Tag with infinite-scroll
```

## Commencer rapidement avec JHipster

Bien que ce post ne soit pas un guide de démarrage, vous pouvez rapidement démarrer avec JHipster de la manière suivante :

```
Installez Java, Git et Node.js, si ce n'est pas déjà fait sur votre machine.
Installez JHipster en exécutant la commande : npm install -g generator-jhipster.
Créez un nouveau répertoire et allez-y : mkdir myApp && cd myApp.
Exécutez JHipster et suivez les instructions à l'écran : jhipster.
Modélisez vos entités avec JDL Studio et téléchargez le fichier .jh résultant.
Générez vos entités avec la commande : jhipster import-jdl <VOTRE_FICHIER_JDL>.jh.
Notez que .jdl et .jh sont tous deux pris en charge.
```

**Modifier les entités existantes**

Supposons que vous souhaitiez modifier

```jdl

entity Tag {
  name String required minlength(2)
}
```

```jdl

entity Tag {
  name String required minlength(3)
}
```

Modifiez le fichier .jdl

Exécutez la commande : 
```
mvnw clean
```
Exécutez la commande : 
```
jhipster import-jdl <VOTRE_FICHIER_JDL>.jdl
```

Répondez aux invites.
Exécutez la commande : 
```
mvnw
```
Vous pouvez rencontrer l'erreur suivante :

```arduino

Application run failed
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'liquibase' defined in class path resource [com/myapplication/config/LiquibaseConfiguration.class]: Invocation of init method failed; nested exception is liquibase.exception.ValidationFailedException: Validation Failed:
     1 change sets check sum
          config/liquibase/changelog/20200808122400_added_entity_Tag.xml::20200808122400-1::jhipster was: 8:a970f5129cedf190a460aca4917f56c5 but is now: 8:13347cabb349df27960176ab29f30179
```

Avant de résoudre l'erreur, voici quelques notes.

Note : Utilisez mvnw pour les machines Windows et ./mvnw pour Linux.

JHipster utilise Liquibase pour effectuer et suivre les modifications du domaine. N'éditez pas les fichiers d'entité directement car les modifications peuvent perturber Liquibase.

**Résoudre les erreurs survenues lors de la modification des entités existantes
Problème :**

Lorsque vous utilisez Liquibase, toutes les modifications d'entité ultérieures doivent être capturées dans des changelogs distincts. Cependant, JHipster écrase les fichiers de changelog. Par conséquent, le checksum du fichier Liquibase de l'entité est modifié car il a maintenant un nouveau contenu. Et dans votre base de données, il y a une table appelée DATABASECHANGELOG qui stocke tous les changelogs appliqués, y compris le checksum.

Résultat : lorsque vous démarrez votre application, vous obtenez une liquibase.exception.ValidationFailedException car le checksum du dernier changelog Liquibase de l'entité modifiée est différent de la dernière fois que vous l'avez exécuté.

Courir la commande 

**mvn liquibase:clearCheckSums n'est pas la bonne approche la plupart du temps**. Cela efface en fait tous les checksums dans la base de données, ce qui signifie que vous perdez le suivi des modifications qui ont eu lieu, ce qui n'est généralement pas l'intention. Cette fonctionnalité de Liquibase a du sens, par exemple, lorsque vous voulez annuler les nouvelles modifications que vous avez appliquées. Si vous effacez les checksums et que vous exécutez l'application, de nouveaux checksums seront calculés ; vous perdez le suivi et cela peut entraîner des problèmes si vous ne faites pas attention.

**Solutions :**

Vérifiez le fichier de changelog pour l'entité. Dans ce cas, l'entité que nous avons éditée était Tag et le fichier Liquibase est situé à : src/main/resources/config/liquibase/changelog/20200808122400_added_entity_Tag.xml.

Revenez en arrière dans le fichier avant que les modifications ne soient apportées. Git est utile pour cela.

Exécutez 

```
mvn compile liquibase:diff
```

 pour capturer les modifications apportées à l'entité dans Liquibase. Ou vous pourriez ajouter les modifications manuellement.

La commande que vous avez exécutée, c'est-à-dire liquibase:diff, génère un fichier de changelog dans le dossier src/main/resources/config/liquibase/changelog/. Vérifiez que le fichier de changelog contient la modification telle qu'elle est. Vous pouvez également éditer ce fichier manuellement.

Ajoutez le fichier de changelog au fichier principal situ
