# mettre en place un déploiement automatique HTML/CSS avec GitLab CI
> Générer par ChatGPT

## 🛠 Étape 1 — Préparer ton environnement GitLab

🎯 Objectif : savoir où et comment ton code va s’exécuter.

Exercice :

    Crée un dépôt GitLab vide appelé site-html-ci.

    Ajoute un fichier index.html avec juste Hello World.

    Lis la doc GitLab CI sur le .gitlab-ci.yml :
    https://docs.gitlab.com/ee/ci/yaml/

    Crée un .gitlab-ci.yml minimal qui affiche juste echo "CI fonctionne !".

💡 Indice : utilise un seul job avec script.
## 🗝 Étape 2 — Comprendre et tester SSH

🎯 Objectif : établir une connexion sécurisée vers ton serveur.

Exercice :

    Sur ton PC, crée une clé SSH :

ssh-keygen -t ed25519 -C "gitlab-ci"

Copie ta clé publique sur ton serveur :

ssh-copy-id user@IP_DU_SERVEUR

Vérifie que la connexion marche sans mot de passe :

    ssh user@IP_DU_SERVEUR

    Lis la doc : https://docs.gitlab.com/ee/user/ssh.html

## 📂 Étape 3 — Apprendre à transférer un site

🎯 Objectif : envoyer des fichiers HTML/CSS vers /var/www/html manuellement.

Exercice :

    Utilise rsync :

rsync -avz ./ user@IP_DU_SERVEUR:/var/www/html/

Teste aussi avec scp :

    scp -r ./ user@IP_DU_SERVEUR:/var/www/html/

    Vérifie dans ton navigateur que ton site a changé.

## 🤖 Étape 4 — Automatiser avec GitLab CI

🎯 Objectif : que GitLab CI fasse le transfert pour toi.

Exercice :

    Ajoute ta clé privée SSH dans GitLab (Settings → CI/CD → Variables).

        Nom : SSH_PRIVATE_KEY

        Valeur : contenu de ~/.ssh/id_ed25519

        Masquer la variable (Masked).

    Dans .gitlab-ci.yml, ajoute un job deploy qui :

        Installe rsync

        Configure la clé SSH depuis la variable

        Fait le transfert

    Déclenche le job en poussant sur main.

## 🌍 Étape 5 — Sécuriser et améliorer

🎯 Objectif : rendre le déploiement fiable.

Exercice :

    Utilise only: [main] pour ne déployer que depuis la branche principale.

    Ajoute un test HTML (ex. grep "<html>" index.html) avant de déployer.

    Ajoute un cache GitLab CI si tu as beaucoup de fichiers.
