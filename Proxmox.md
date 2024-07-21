
## Installer Proxmox sur un Mini PC
---
### Processeur N100 Adler Lake
---
Lancer l'ISO et attendre le message **Installation aborted**

Dans le terminal entrer les commandes suivantes :
```bash
cd /usr/share/X11/xorg.conf.d/
```

Une fois dans le répertoire, créer un nouveau fichier de conf.
```bash
nano 00-intel.conf
```

Faire un CTRL+Z pour sortir du fichier et regarder les périphériques PCI du pc.
```bash
lspci
```

Retourner dans le fichier de conf avec la commande *`fg`* et inscrire les informations suivantes (attention le clavier est en QWERTY):
```bash
Section "Device"
	Identifier "Card0"
	Driver "fbdev"
	BusID "pci0:00:0:2:"
EndSection
```

Sauvegarder et quitter puis relancer la suite de l'installation de Proxmox.
```bash
xinit -- -dpi 96 >/dev/tty2 2>&1
```

⚠️ Attention :
**Si un nouveau Proxmox doit être installé, cette manipulation doit être faite de nouveau.**
### Mise à jour de Proxmox (7 to 8)
---
Une fois connecté à la console de Proxmox, se rendre dans le fichier contenant la liste des répertoires de mise à jour.
```bash
nano /etc/apt/sources.list
```

Ajouter une ligne pointant vers la dernière version de Proxmox 7.
```bash
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

Enlever le fichier contenant les répertoires de mise à jour pour les entreprises (abonnement payant).
```bash
rm /etc/apt/sources.list.d/pve-enterprise.list
```

Mettre à jour le serveur.
```bash
apt update && apt upgrade -y && apt dist-upgrade -y
```

Vérifier la possibilité de passer vers la version supérieure.
```bash
pve7to8 --full
```

Changer les `bullseye` en `bookworm` dans le fichier de répertoires.
```bash
sed -i 's/bullseye/bookworm/g' /etc/apt/sources.list
```

Mettre de nouveau à jour le serveur (plusieurs choix à faire au cours de l'installation).
```bash
apt update && apt upgrade -y
```

Redémarrer le serveur
```bash
reboot
```

#### Changer le Kernel
---
Il se peut que certains serveurs ne prennent pas le kernel le plus récent et ne puissent pas démarrer. 

Spécifier le Kernel voulu.
```bash
pve-efiboot-tool kernel pin 5.15.102-1-pve
```

*Optionnel* : Vérifier la liste.
```bash
pve-efiboot-tool kernel list
```

### Modifications éléments Proxmox

