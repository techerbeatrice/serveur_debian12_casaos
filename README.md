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
<img width="1050" height="850" alt="image" src="https://github.com/user-attachments/assets/c7d970fa-261f-4404-9ea8-d62d5d8683cf" />

_____

# D) Télécharger des applications dans l'App Store de CasaOS :  


<img width="1021" height="852" alt="image" src="https://github.com/user-attachments/assets/b27bc326-7a18-474a-b8b4-6f3bb0316ae3" />

