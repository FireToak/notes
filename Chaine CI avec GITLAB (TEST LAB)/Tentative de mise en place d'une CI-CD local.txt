# **Tentative** de mise en place d'une chaîne CI sur l'infra Loutik - 09/08/2025

## A. Installation de GitLab :

1. Ajout du repo GitLab CE sur Debian :
```
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```
[Version de GitLab CE en fonction de votre système](https://packages.gitlab.com/gitlab/gitlab-ce)

2. Mettre à jour le système avant d'installer GitLab
```
sudo apt update && sudo apt upgrade
```

3. Installer le package GitLab Omnibus en configurant les variables :
```
sudo GITLAB_ROOT_EMAIL="louismedo@ik.me" GITLAB_ROOT_PASSWORD="PommeDeTerre58!" EXTERNAL_URL="http://gitlab.loutik.lab" apt install gitlab-ce
```

Source : 
- https://docs.gitlab.com/install/package/debian/
- https://packages.gitlab.com/gitlab/gitlab-ce
- https://blog.stephane-robert.info/docs/services/devops/gitlab/installation/

## B. Installation GitLab runner :

1. Ajout du repo GitLab runner sur Debian :
```
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```
-L => Permer d'indiquer à curl de suivre les redirections. Si le serveur répond avec une redirection (par exemple, un code de statut HTTP 301 ou 302), curl va automatiquement suivre cette redirection vers la nouvelle URL.

2. Installer GitLab runner :
```
sudo apt update && sudo apt upgrade
sudo apt install -y gitlab-runner
```

source :
- https://docs.gitlab.com/runner/install/linux-repository/

## C. Lié une instance GitLab ce à un runner :

1. Aller dans le panneau administrateur (Bouton Admin en bas à gauche de la page)

2. Rendez-vous dans CI/CD > Runners, puis "create instance runner"

3. Remplir les champs

4. Copier coller la step 1 dans votre instance GitLab runner
```
gitlab-runner register  --url http://gitlab.loutik.lab  --token glrt-ku_I8qM5DS8hwAiJJXP-Bm86MQp0OjEKdToxCw.01.121gzg2s2
```

5. Compléter par vos choix pour le runner
Mes choix :
```
Enter a name for the runner. This is stored only in the local config.toml file:
[gitlab-runner]: gitlab-runner01
Enter an executor: custom, parallels, virtualbox, docker+machine, kubernetes, docker-autoscaler, instance, shell, ssh, docker, docker-windows:
docker
Enter the default Docker image (for example, ruby:3.3):
alpine:latest
```
