# Création de l'instance Runner de GitLab
> Permet de faire les tests, build et d'envoyer le code sur les serveurs

1. Installation de gitlab-runner sur le serveur :
```
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

2. Enregistrer le gitlab-runner sur l'instance gitlab
```
sudo gitlab-runner register
```

3. Répondre aux questions :
Exemple :
```
[sudo] Mot de passe de user :
Runtime platform                                    arch=amd64 os=linux pid=1129 revision=cc489270 version=18.2.1
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
http://gitlab.loutik.lab
Enter the registration token:
glrt-jPAAo72kN75kppcwZAPTtG86MQp0OjEKdToxCw.01.1207leiqm
Verifying runner... is valid                        correlation_id=01K2CNG7H6S16N2AAJCCGCXW03 runner=jPAAo72kN
Enter a name for the runner. This is stored only in the local config.toml file:
[gitlab-runner]: website
Enter an executor: ssh, parallels, virtualbox, docker+machine, shell, docker, docker-windows, kubernetes, docker-autoscaler, instance, custom:
ssh
Enter the SSH server address (for example, my.server.com):
192.168.1.13
Enter the SSH server port (for example, 22):
22
Enter the SSH user (for example, root):
user
Enter the SSH password (for example, docker.io):
admin
Enter the path to the SSH identity file (for example, /home/user/.ssh/id_rsa):
/home/user/.ssh/gitlab-runner
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!

Configuration (with the authentication token) was saved in "/etc/gitlab-runner/config.toml"
```