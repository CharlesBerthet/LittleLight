## Installation de Fail2Ban
---
Mettre à jour le serveur.
```bash
apt update && apt upgrade -y
```

Installer le module Fail2ban.
```bash
apt install fail2ban
```

Rendre permanant l'activation de Fail2ban.
```bash
Systemctl start fail2ban
Systemctl enable fail2ban
```

Dupliquer le fichier de conf pour en avoir un local.
```bash
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

Se rendre dans le nouveau fichier.
```bash
nano /etc/fail2ban/jail.local
```

Aller a la fin et copier la configuration donner par Proxmox : https://pve.proxmox.com/wiki/Fail2ban
```bash
[proxmox]
enabled = true
port = https,http,8006
filter = proxmox
backend = systemd
maxretry = 3
findtime = 2d
bantime = 1h
```

Redémarrer le service Fail2ban.
```bash
systemctl restart fail2ban
```

*Optionnel* : Vérifier le statut de Fail2ban.
```bash
systemctl status fail2ban
```

