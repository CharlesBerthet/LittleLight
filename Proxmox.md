
## Installer Proxmox sur un Mini PC
----

### Processeur N100 Adler Lake
---
Lancer l'ISO et attendre le message **Installation aborted**

Dans le terminal entrer les commandes suivantes :
```
cd /usr/share/X11/xorg.conf.d/
```

Une fois dans le répertoire, créer un nouveau fichier de conf.
```
nano 00-intel.conf
```

Faire un CTRL+Z pour sortir du fichier et regarder les périphériques PCI du pc.
```
lspci
```

Retourner dans le fichier de conf avec la commande *`fg`* et inscrire les informations suivantes (attention le clavier est en QWERTY):
```
Section "Device"
	Identifier "Card0"
	Driver "fbdev"
	BusID "pci0:00:0:2:"
EndSection
```

Sauvegarder et quitter puis relancer la suite de l'installation de Proxmox.
```
xinit -- -dpi 96 >/dev/tty2 2>&1
```

⚠️ Attention :
**Si un nouveau Proxmox doit être installé, cette manipulation doit être faite de nouveau.**
### aze


```
aze
```

