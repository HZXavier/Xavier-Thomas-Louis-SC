# Sécurité des Containers - Projet

**Auteurs:** Xavier HEITZ, Thomas LEPEU & Louis DECOURTIS

## Lien vers le projet GitLab
[Minecraft Secure Cluster](https://gitlab.com/securitecontaienr/minecraft-secure-cluster.git)

## Présentation du projet

Ce projet est une compilation des travaux pratiques réalisés dans le cadre du cours sur la sécurité des containers. Il aborde les différentes facettes de la sécurité dans l'écosystème des containers, de Docker aux orchestrateurs comme Kubernetes, en incluant également les aspects CI/CD.

## Table des matières

1. [Session 1: Introduction aux containers et à la sécurité](#session-1-introduction-aux-containers-et-à-la-sécurité)
2. [Session 2: Restrictions d'accès et audit](#session-2-restrictions-daccès-et-audit)
3. [Session 3: Kubernetes et sécurité des orchestrateurs](#session-3-kubernetes-et-sécurité-des-orchestrateurs)
4. [Session 4: Sécurité dans la CI/CD](#session-4-sécurité-dans-la-cicd)

## Session 1: Introduction aux containers et à la sécurité

Cette session présente les bases des containers Docker et aborde les concepts fondamentaux de sécurité:

- Lancement de containers simples et interactifs
- Analyse des ressources système des containers
- Exploration des capacités des containers
- Tests de containers avec permissions élevées
- Simulation d'évasion de container
- Création d'images sécurisées
- Restrictions d'accès réseau
- Scan d'images avec Trivy et Grype

[Voir la documentation complète de la Session 1](./mini%20projet/Session_1.md)

## Session 2: Restrictions d'accès et audit

Cette session se concentre sur les aspects pratiques de la sécurité des containers:

- Restriction des ports exposés
- Limitation des permissions d'accès aux fichiers sensibles
- Audit de configuration avec Docker Bench
- Gestion sécurisée des secrets avec Vault
- Identification de clés API cachées dans les images

[Voir la documentation complète de la Session 2](./mini%20projet/Session_2.md)

## Session 3: Kubernetes et sécurité des orchestrateurs

Cette session explore l'écosystème Kubernetes et les enjeux de sécurité associés:

- Architecture et composants principaux de Kubernetes
- Enjeux de sécurité des orchestrateurs
- Configuration RBAC (Role-Based Access Control)
- Déploiement d'un cluster avec Kind
- Expérimentation des RBAC
- Scan de sécurité avec Kube-Bench
- Détection d'intrusions avec Falco

[Voir la documentation complète de la Session 3](./mini%20projet/Session_3.md)

## Session 4: Sécurité dans la CI/CD

Cette session porte sur l'intégration des pratiques de sécurité dans les pipelines CI/CD:

- Signature d'images avec Cosign
- Test de vérification d'intégrité des images
- Configuration de pipelines GitLab CI
- Mise en place de scans automatisés (Hadolint, Trivy)
- Vérification de signatures dans le pipeline

[Voir la documentation complète de la Session 4](./mini%20projet/Session_4.md)

## Résultats et conclusions

Ce projet illustre l'importance d'adopter une approche de sécurité à plusieurs niveaux dans l'écosystème des containers:

1. **Sécurisation des images:** Utilisation d'images minimales, sans vulnérabilités connues et configuration de l'utilisateur non-root.
2. **Sécurisation des containers:** Limitation des capacités, restrictions réseau et isolation des ressources.
3. **Sécurisation des orchestrateurs:** Configuration RBAC, application des politiques de sécurité et surveillance des comportements anormaux.
4. **Sécurisation de la chaîne d'approvisionnement:** Signature des images, vérification d'intégrité et scans automatisés.

L'ensemble de ces pratiques permet de réduire significativement la surface d'attaque et de minimiser les risques de sécurité lors de l'utilisation des technologies de conteneurisation.

## Ressources supplémentaires

- [Docker Security Documentation](https://docs.docker.com/engine/security/)
- [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/security-checklist/)
- [CIS Docker Benchmark](https://www.cisecurity.org/benchmark/docker)
- [CIS Kubernetes Benchmark](https://www.cisecurity.org/benchmark/kubernetes)
