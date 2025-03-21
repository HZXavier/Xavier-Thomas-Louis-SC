# Session 1
Introduction aux Containers et à la Sécurité

1. Lancer un container simple

Apres avoir installer docker nous executons la commande suivante :
```bash
docker run --rm hello-world
```
Cette commade telecharger et execute l'iamge hello-world
![Resultat de la commande hello-world](./Image%20session%201/hello-world.png)

2. Explorer un container en interactif 

Nous executons  la commande suivante : 
```bash
docker run -it --rm alpine sh
```
"-it" nous permet de lancer une session interactive avec un shell via lequel nous allons tester des commandes Linux de bases (ls,pwd,whoami)

![Resultat de la commande alpine sh](./Image%20session%201/alpine%20cmd.png)

3. Analyser les ressources systeme d'un container 

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


