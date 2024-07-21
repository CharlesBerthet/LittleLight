## Mise en place de Nginx
---
### Installation de Docker
---
Tout d'abord, mettre à jour la machine.
```bash
sudo apt update
sudo apt upgrade -y
```

Puis installer les paquets pour Docker.
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Récupérer la clé GPG de Docker.
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Ajouter le dépôt de Docker APT.
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Mettre de nouveau à jour la machine.
```bash
sudo apt update
```

Installer Docker et Docker Compose.
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugi
```

*Optionnel* : Vérifier le statut de Docker.
```bash
sudo systemctl status docker
```


### Installation de Nginx Proxy Composer
---
Créer un dossier pour ranger le Docker de NPM.
```bash
mkdir -p ~/nginx-proxy-manager
```

Se rendre dans le dossier.
```bash
cd ~/nginx-proxy-manager
```

Créer un fichier `yml`.
```bash
nano docker-compose.yml
```

Coller le contenu donner par le site : https://nginxproxymanager.com/setup/
```yml
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    # Uncomment the next line if you uncomment anything in the section
    # environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

Démarrer le Docker Nginx.
```bash
sudo docker compose up -d
```

*Optionnel* : Vérifier le conteneur.
```bash
sudo docker ps
```

### Site Web
---
Adresse du site :
http://<@IP de la machine>:81

Credentials :
- Email : `admin@example.com`
- MDP : `changeme`