# serveur_debian12_casaos

________________


# 🔴 Prérequis

PC avec debian 12


<img width="1118" height="828" alt="image" src="https://github.com/user-attachments/assets/8d726e39-7495-44a2-811d-84267d9c2d23" />

____________________________________

# 🔴 Empêcher le serveur de se mettre en veille

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
si ça répond → victoire    

_____

# 🔴 Sécuriser le serveur   

dans le terminal : 

```
# Mises à jour automatiques de sécurité
sudo apt install unattended-upgrades apt-listchanges
sudo dpkg-reconfigure -plow unattended-upgrades

# Firewall strict (ufw)
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp  # SSH - changez le port !
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# Fail2ban contre bruteforce
sudo apt install fail2ban
sudo systemctl enable fail2ban
```

SSH : Changez le port (réduit les scans), désactivez root, utilisez clés ED25519 uniquement :  

```
sudo nano /etc/ssh/sshd_config
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Génération de la paire de clés (sur votre ordinateur client) :  

```  
# Option recommandée : ED25519 (rapide et sécurisé)
ssh-keygen -t ed25519 -a 100 -C "votre-email@exemple.com"

# Alternative si le serveur est ancien : RSA 4096 bits
ssh-keygen -t rsa -b 4096 -C "votre-email@exemple.com"
```   
Transfert de la clé publique vers le serveur (sur votre ordinateur client) :   

```  
# Si vous avez changé le port SSH (ex: 2222)
ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 2222 utilisateur@IP_SERVEUR
```
Entrez votre mot de passe actuel une dernière fois. La clé est ajoutée à \~/.ssh/authorized_keys.   

Si ça ne fonctionne pas (si ssh-copy-id indisponible) :    

```   
# Sur le client
cat ~/.ssh/id_ed25519.pub | ssh utilisateur@IP_SERVEUR "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```   
 
_____

# 🔴 Installation de CasaOS (gestionnaire de serveur)   

télécharger CasaOS en ligne de commande :   
```
curl -fsSL https://get.casaos.io | sudo bash”
``` 
<img width="1100" height="920" alt="image" src="https://github.com/user-attachments/assets/6acfbdb3-a3c4-41ee-8908-fac3453d7d96" />

_____

# 🔴 Télécharger des applications dans l'App Store de CasaOS  


<img width="1021" height="852" alt="image" src="https://github.com/user-attachments/assets/b27bc326-7a18-474a-b8b4-6f3bb0316ae3" />

_____

🟢 **Nextcloud dans l'App Store de CasaOS :**
Nextcloud est un logiciel libre de site d'hébergement de fichiers et une plateforme de travail collaboratif.   

<img width="1014" height="857" alt="image" src="https://github.com/user-attachments/assets/77cc6433-93c0-4e7b-a7f8-7d88c8a47639" />

_____

🟢 **Pi-hole dans l'App Store de CasaOS :**
Pi-hole est un bloqueur de publicité au niveau du réseau qui agit comme un DNS menteur et éventuellement comme un serveur Dynamic Host Configuration Protocol, destiné à être utilisé sur un réseau privé.   

<img width="1023" height="866" alt="image" src="https://github.com/user-attachments/assets/ec9c71d2-e7e2-4124-aad3-0146f4a14272" />

_____

🟢 **Immich dans l'App Store de CasaOS :**
Immich, une alternative gratuite, open-source et auto-hébergé à Google Photos

<img width="1015" height="852" alt="image" src="https://github.com/user-attachments/assets/82c68ce4-a1e6-40b5-9bc5-273137b05c4e" />

_____

🟢 **Portainer dans l'App Store de CasaOS :**
Portainer est un puissant outil de gestion Docker. Dans toute l'interface Web, Portainer facilite la gestion des applications et des images Docker pour ceux qui ne sont pas familiarisés avec les commandes Docker, ce qui facilite son utilisation.

<img width="958" height="679" alt="image" src="https://github.com/user-attachments/assets/d3059999-d9c3-446d-a046-17acdb8a2304" />

