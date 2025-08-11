## Etape 1 - Préparer ton environnement GitLab
> Crée un dépôt GitLab vide appelé site-html-ci.
> 
> Ajoute un fichier index.html avec juste Hello World.
> 
> Lis la doc GitLab CI sur le .gitlab-ci.yml :
> https://docs.gitlab.com/ee/ci/yaml/
> 
> Crée un .gitlab-ci.yml minimal qui affiche juste echo "CI fonctionne !".

J'ai créé un répo sur gitlab avec fichier index.html, et j'ai ensuite créé un fichier .gitlab-ci.yml avec le contenu :
```
stages: #Les étapes
- test #Nom de l'étape

test: # On déclare l'étape ici
  tags: # On déclare le tag, c'est ce qui va dire à gitlab sur quel serveur aller pour faire les tests et déployer le code
  - website # Nom du Runner gitlab pour déployer mon code HTML
  script: # Exécute des scripts bash
  - echo "Mon message" # On peut utiliser echo qui va envoyer un message dans la console
```

## Étape 2 — Comprendre et tester SSH

Ce que j'ai fait :
1. Création de la clé ssh avec ssh-keygen
2. Copie de la clé ssh publique sur le serveur web dans le fichier authorized_keys (toujours dans un répertoire .ssh)
3. Création du fichier "config" dans .ssh
```
Host 192.168.1.13
  HostName 192.168.1.13
  IdentityFile ~/.ssh/deploy-website
  User user
```
4. Tester la configuration avec l'envoi d'un fichier html vers le répertoire "sites" du serveur web
```
scp index.html user@web:/home/sites/eliot-website
```

## Étape 4 — Automatiser avec GitLab CI

1. Création du fichier .gitlab-ci.yml

2. Écriture du fichier
```
stages:
  - deploy # Étape de déploiement

deploy_job: # Job de déploiement
  tags:
    - website
  stage: deploy # Ce job appartient à l'étape "deploy"
  script:
    # Créer le dossier .ssh à la racine de l'utilisateur
    - mkdir -p ~/.ssh

    # Ajoute la clé SSH privée dans le dossier .ssh
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_ed25519

    # Change les permissions pour que seulement l'utilisateur ait accès à la clé (Sinon on ne peut pas utiliser la clé, SSH bloque par mesure de sécurité)
    - chmod 600 ~/.ssh/id_ed25519

    # Ajoute le serveur distant à known_hosts pour éviter la question interactive
    - ssh-keyscan -H 192.168.1.13 >> ~/.ssh/known_hosts

    # Copie tout le code dans le dossier temporaire de Gitlab vers le serveur WEB.
    - scp -r $CI_PROJECT_DIR/* user@192.168.1.13:/home/sites/eliot-website

  only:
    - main # Ce job ne se déclenche que si la branche "main" est mise à jour
```
