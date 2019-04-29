# docker-tomcat-spring-example

Ce dépôt est un exemple de dockerisation d'une application sprint-boot compilée dans un war via [l'image docker officielle maven](https://hub.docker.com/_/maven) puis déployée et exécutée dans un [tomcat via son image docker officielle](https://hub.docker.com/_/tomcat).

La fonctionnalité [multistage-build de docker](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) est ici utilisée pour pouvoir compiler le WAR dans un premier conteneur docker indépendant puis de le déployer (sans son environnement de build) dans un autre conteneur qui est lui uniquement chargé d'exécuter un tomcat.

# Construire l'application

```shell
docker-compose -f docker-compose.build.yml build
```

L'image docker `docker-tomcat-spring-example:1.0.0` sera alors construite localement. A noter que si les source Java de l'application (répertoire `src/`) sont modifiées, les dépendances maven ne seront pas retéléchagées (ce qui prend du temps) car elles ont été mise en cache par le système multistage-build de docker. Le dépendances seront retéléchargées uniquement si `pom.xml` est modifié.

# Exécuter l'application

```shell
docker-compose up
```

Le conteneur docker nommé `docker-tomcat-spring-example` est alors créé à partir de l'image docker`docker-tomcat-spring-example:1.0.0` préalablement construite. Le serveur web (tomcat + spring boot) est alors à l'écoute sur le port local 8080 : http://localhost:8080/

Pour stopper l'application utiliser `CTRL+C`