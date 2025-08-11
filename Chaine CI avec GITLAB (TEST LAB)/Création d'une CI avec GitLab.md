# Création d'une CI avec GitLab

1. Création du fichier `.gitlab-ci.yml`

2. Écriture du fichier gitlab-ci  
```
stages:
  - deploy # Étape de déploiement

deploy_job: # Job de déploiement
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

    # Copie tout le code dans le dossier temporaire de GitLab vers le serveur WEB.
    - scp -r $CI_PROJECT_DIR/* user@192.168.1.13:/home/sites/eliot-website

  only:
    - main # Ce job ne se déclenche que si la branche "main" est mise à jour
```

#### Note :

J'ai créé un Runner avec comme executor shell, je n'ai pas besoin d'utiliser des conteneurs pour mes sites en HTML, je veux juste que les fichiers soient transmis directement sur le serveur web distant.

#### source : 
- https://docs.gitlab.com/ci/yaml/
- https://docs.gitlab.com/ci/
- https://docs.gitlab.com/ci/quick_start/
- https://docs.gitlab.com/ci/yaml/#stage