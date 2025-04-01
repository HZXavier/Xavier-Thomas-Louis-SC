# Session 1 : Introduction aux containers et a la securite

## Activites pratiques 1

# 1. Lancer un container simple

Apres avoir installer docker nous executons la commande suivante :
```bash
docker run --rm hello-world
```
Cette commade telecharger et execute l'iamge hello-world
![Resultat de la commande hello-world](./Image%20session%201/hello-world.png)

# 2. Explorer un container en interactif 

Nous executons  la commande suivante : 
```bash
docker run -it --rm alpine sh
```
"-it" nous permet de lancer une session interactive avec un shell via lequel nous allons tester des commandes Linux de bases (ls,pwd,whoami)

![Resultat de la commande alpine sh](./Image%20session%201/alpine%20cmd.png)

# 3. Analyser les ressources systeme d'un container 

Nous executons la commande suivante :
```bash
docker run -d --name test-container nginx
```
![Resultat de la commande test-container nginx](./Image%20session%201/test-container-nginx.png)

Cela nous permet de lamcer un container base sur l'image nginx
Puis nous executons la commande suivante :
```bash
docker stats test-container
```
![Resultat de la commande stats test-container ](./Image%20session%201/test-container-stats.png)

Cela nous affiche en temos reel la consommation CPU, RAM, Reseau, disque.

# 4. Lister les capacites d'un container 

En executant la commande suivante :
```bash
docker run --rm --cap-add=SYS_ADMIN alpine sh -c 'cat /proc/self/status'
```
Nous allons temporairement ajouter une **capacité spéciale : `SYS_ADMIN`** pour y observer les changements.

![Resultat de la commande capa ](./Image%20session%201/capabilities.png)

Le format CapEff nous donnes les capabilities effectives, dans notres cas elle contient maintenant la capacite SYS_ADMIN representee par 00000000a82425fb
On comprend que Docker limite par défaut les permissions d’un conteneur, et qu’il est possible d’ajouter des droits spécifiques pour lui donner plus de privilèges si nécessaire.

## Activites pratiques 2

# 1. Tester un Container avec des Permissions Élevées

Nous lancons un container en mode privilégié et executons une commande système :
```bash
docker run --rm --privileged alpine sh -c 'echo hello from privileged mode'
```
![Resultat de la commande privi ](./Image%20session%201/privileged-mode.png)

Le conteneur affiche correctement le message: "hello from privileged mode"

Lancer un conteneur avec l’option --privileged est une pratique dangereuse car elle supprime la plupart des mécanismes d’isolation entre le conteneur et la machine hôte. 

Cette option accorde au conteneur un accès complet aux périphériques du système, à toutes les capacités Linux, et désactive des protections essentielles comme AppArmor ou SELinux. 

En cas de compromission, un attaquant pourrait ainsi s’échapper du conteneur, accéder aux fichiers sensibles de l’hôte, et potentiellement en prendre le contrôle total.

# 2. Simuler une Évasion de Container

Nous exécutons la commande suivante :
```bash
docker run --rm -v /:/mnt alpine sh -c 'ls /mnt'
```
![Resultat de la commande / ](./Image%20session%201/acces-ls.png)

Cette commande montre bien que le conteneur peut lister tout le système de fichiers de l’hôte.

Monter le système de fichiers de l’hôte (/) dans un conteneur représente une faille de configuration majeure, car cela supprime toute isolation entre le conteneur et la machine physique. 

Cette pratique expose l’hôte à de nombreux risques : le conteneur peut lire ou modifier des fichiers sensibles, compromettre la confidentialité des données, altérer l’intégrité du système, contourner les mécanismes de sécurité comme AppArmor ou SELinux, voire installer des malwares ou obtenir un accès root sur l’hôte.

# 3. Créer une Image Sécurisée

Dockerfile utilisé:
```bash
FROM alpine
RUN adduser -D appuser
USER appuser
CMD ["echo", "Container sécurisé!"]
```
Construction et exécution de l’image :
![Resultat de la commande build & run ](./Image%20session%201/dockerfile-build&run.png)

Vérification de l'UID et du nom d'utilisateur dans le conteneur :
![Resultat de la commande UID ](./Image%20session%201/UID.png)

En utilisant un Dockerfile minimaliste, nous avons construit une image sécurisée qui suit les bonnes pratiques de sécurité Docker.Le fichier définit la création d’un utilisateur non-root (appuser) et configure le conteneur pour qu’il s’exécute sous cet utilisateur plutôt qu’en tant que root. 

Cette approche permet de réduire la surface d’attaque du conteneur, car même en cas de compromission, un attaquant n’aurait pas les privilèges nécessaires pour interagir avec des éléments sensibles du système hôte.

# 4 & 5. Restreindre l’accès réseau d’un container & Bloquer la connexion internet dans un container :

Pour bloquer la connexion réseau d’un conteneur Docker, notamment l’accès à Internet, nous utilisons la commande suivante :
```bash
docker network disconnect bridge test-container
```

Cette commande déconnecte le conteneur nommé test-container du réseau bridge par défaut, ce qui coupe toute communication réseau, y compris vers l’extérieur.

# 6. Tester l’accès internet avec par exemple ping google.com.

Pour vérifier que la coupure était effective, nous avons lancé une commande ping depuis le conteneur :
![Resultat de la commande UID ](./Image%20session%201/acces-reseau.png)

Le test a échoué, confirmant que le conteneur n’avait plus accès à Internet.

# 7. Télécharger et Scanner une Image

On commence par récupérer une image volontairement vulnérable :
```bash
docker pull vulnerables/web-dvwa
```

Puis on scanne cette image avec Trivy :
```bash
trivy image vulnerables/web-dvwa
```

Et enfin on sauvegarde  le resultat de ce scan dans un fichier JSON :
```bash
trivy image -f json -o resultats-trivy.json vulnerables/web-dvwa
```
Voici un rapide résumé des vulnérabilités de cette image :

L’image scannée repose sur Debian 9.5, un système obsolète et non maintenu, ce qui représente un risque majeur en soi. Le scan a révélé un grand nombre de vulnérabilités, parmi lesquelles on retouvre :
*CVE-2021-26691 : Dépassement de mémoire dans le module mod_session d’Apache2.
*CVE-2021-44790 : Débordement de tampon dans mod_lua, exploitable via une requête multipart malveillante.
*CVE-2022-22720 / 22721 : Vulnérabilités liées à la manipulation des requêtes HTTP dans Apache, pouvant entraîner des attaques de type HTTP request smuggling ou corruption mémoire.

# 8. Scanner une Image pour Détecter les Vulnérabilités

Analyse d'une image avec Grype :
```bash
grype alpine:latest
```
![Resultat de la commande grype ](./Image%20session%201/Grype.png)

Grype a détecté aucune vulnérabilités, ce qui est cohérent avec l’objectif d’Alpine : être une image minimaliste et sécurisée.

Le scan avec Trivy et Grype a révélé que web-dvwa contient de nombreuses vulnérabilités critiques, notamment dans Apache2 et OpenSSL, liées à l’utilisation d’un système obsolète (Debian 9.5). À l’inverse, l’image Alpine est extrêmement légère et à jour, ce qui limite fortement la surface d’attaque. Très peu de vulnérabilités y ont été détectées, et aucune critique.

Differences entre Grype et Trivy :

Grype et Trivy sont deux outils d’analyse de vulnérabilités dans les images Docker. Trivy est plus complet : il détecte les vulnérabilités, les secrets (clés API, mots de passe) et les erreurs de configuration, tout en étant rapide et simple à intégrer dans les pipelines CI/CD. Grype, plus spécialisé, offre une détection fine des vulnérabilités système et fonctionne bien avec Syft pour générer des SBOM. 