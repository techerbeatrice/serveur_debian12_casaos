# serveur_debian12_casaos

________________


# A) Prérequis :

PC avec debian 12


<img width="1118" height="828" alt="image" src="https://github.com/user-attachments/assets/8d726e39-7495-44a2-811d-84267d9c2d23" />

____________________________________

# B) Empêcher le serveur de se mettre en veille :

dans le terminal : 

```
systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target
```
```
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
```
systemctl is-enabled suspend.target
```

et en modifiant le fichier :   
```
sudo nano /etc/default/grub  
```
en remplaçant la ligne :
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet"   
```
par
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet processor.max_cstate=1 intel_idle.max_cstate=0"   
```
et en appliquant la modification :   
```
sudo update-grub   
```
```
sudo reboot
```  

test après reboot en laissant le serveur 1–2 heures sans y toucher, puis essayer sur un **pc client** :   
```
ssh user@ip_du_serveur   
```
si ça répond → victoire :

_____


# C) Installation de CasaOS (gestionnaire de serveur) :   

télécharger CasaOS en ligne de commande :   
```
curl -fsSL https://get.casaos.io | sudo bash”
``` 
<img width="1100" height="920" alt="image" src="https://github.com/user-attachments/assets/6acfbdb3-a3c4-41ee-8908-fac3453d7d96" />

_____

# D) Télécharger des applications dans l'App Store de CasaOS :  


<img width="1021" height="852" alt="image" src="https://github.com/user-attachments/assets/b27bc326-7a18-474a-b8b4-6f3bb0316ae3" />

_____

- **Nextcloud dans l'App Store de CasaOS :**
Nextcloud est un logiciel libre de site d'hébergement de fichiers et une plateforme de travail collaboratif.   

<img width="1014" height="857" alt="image" src="https://github.com/user-attachments/assets/77cc6433-93c0-4e7b-a7f8-7d88c8a47639" />

_____

- **Pi-hole dans l'App Store de CasaOS :**
Pi-hole est un bloqueur de publicité au niveau du réseau[2] qui agit comme un DNS menteur et éventuellement comme un serveur Dynamic Host Configuration Protocol, destiné à être utilisé sur un réseau privé.

<img width="1023" height="866" alt="image" src="https://github.com/user-attachments/assets/ec9c71d2-e7e2-4124-aad3-0146f4a14272" />

_____

- **Immich dans l'App Store de CasaOS :**
Immich, une alternative gratuite, open-source et auto-hébergé à Google Photos

<img width="1015" height="852" alt="image" src="https://github.com/user-attachments/assets/82c68ce4-a1e6-40b5-9bc5-273137b05c4e" />
