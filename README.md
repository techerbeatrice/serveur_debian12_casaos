# serveur_debian12_casaos

________________
# Les étapes    

👉  Transformer un pc debian 12 en serveur     
👉  Installer CasaOs sur ce serveur        
👉  Télécharger des applications via CasaOs     
👉  Nextcloud pour héberger de fichiers et une plateforme de travail collaboratif : installation et paramétrage   
👉  Pi-hole pour bloquer les pubs : installation et paramétrage   
👉  Immich pour gérer et sauvegarder des photos en auto-hébergement : installation et paramétrage    
👉  Portainer pour créer, modifier, redémarrer, surveiller… des conteneurs Docker : installation et paramétrage   
👉  Duplicati pour faire des sauvegardes : installation et paramétrage   
👉  Jellyfin pour lire et partager des fichiers multimédias numériques : installation et paramétrage   
👉  Nginx Proxy Manager pour créer, gérer des hôtes virtuels, des redirections, des certificats SSL et des règles de sécurité pour leurs serveurs proxys : installation et paramétrage   
👉  WordPress pour créer et gérer des sites Web : installation et paramétrage   
👉  WatchTower pour les mises à jour automatiques : installation et paramétrage   

_______


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

🔧 **Immich paramétrage :**  

Dans les paramètres :   

<img width="1025" height="596" alt="image" src="https://github.com/user-attachments/assets/aa672ad6-0e69-448f-a5e4-965b8c4cbc5e" />

-------------

🔷 Dans l'onglet database   


<img width="624" height="819" alt="image" src="https://github.com/user-attachments/assets/ec8e8f38-7866-421e-b4c4-1936dce1b7bd" />
<img width="627" height="817" alt="image" src="https://github.com/user-attachments/assets/b51f8415-154c-441f-a73f-f3f9171d147b" />
<img width="621" height="820" alt="image" src="https://github.com/user-attachments/assets/f73309b4-411a-4eb3-8f3a-d4f102709ef0" />

-------------

🔷 Dans l'onglet immich-machine-learning   

<img width="631" height="825" alt="image" src="https://github.com/user-attachments/assets/e698496d-f351-4585-92b1-ea391e9e8b7e" />
<img width="629" height="819" alt="image" src="https://github.com/user-attachments/assets/ba9290b7-feda-4667-a591-59d97a015da4" />
<img width="626" height="833" alt="image" src="https://github.com/user-attachments/assets/aee25d17-5c65-43cb-a2e1-90762c03961d" />


-------------

🔷 Dans l'onglet immich-server   

<img width="625" height="824" alt="image" src="https://github.com/user-attachments/assets/99fae3c9-9d7d-43b1-8859-fadfc3a0afc0" />
<img width="628" height="823" alt="image" src="https://github.com/user-attachments/assets/d4038cbf-88cd-4ce6-a31a-95ef6e519e6e" />
<img width="623" height="824" alt="image" src="https://github.com/user-attachments/assets/82a085cb-14a3-4469-8520-62d4309faa11" />

-------------

🔷 Dans l'onglet redis  

<img width="628" height="821" alt="image" src="https://github.com/user-attachments/assets/666e815c-9675-4cee-96e2-d3cc26a14793" />
<img width="625" height="818" alt="image" src="https://github.com/user-attachments/assets/05020131-839e-48c3-a15f-b65806929441" />

----------

🟦  Enregistrer les paramètres, ouvrir Immich Web, renseigner email + mot de passe             

<img width="843" height="785" alt="image" src="https://github.com/user-attachments/assets/dc08e84a-c18a-4504-a6bd-3c102f5abd12" />

----------

 🟦  Sur votre mobile, télécharger l'application Immich        

<img width="1020" height="848" alt="image" src="https://github.com/user-attachments/assets/953507a8-dff3-4d69-a1af-7ffa7d60248a" />

-----

 🟨 et se connecter à l'application mobile avec l'identifiant de votre Immich Web : **http://IP_DU_SERVEUR:2283** + mot de passe    
 
_____

🟢 **Portainer dans l'App Store de CasaOS :**
Portainer est un puissant outil de gestion Docker. Dans toute l'interface Web, Portainer facilite la gestion des applications et des images Docker pour ceux qui ne sont pas familiarisés avec les commandes Docker, ce qui facilite son utilisation.

<img width="958" height="679" alt="image" src="https://github.com/user-attachments/assets/d3059999-d9c3-446d-a046-17acdb8a2304" />

_____

🟢 **Duplicati dans l'App Store de CasaOS :**
Duplicati est un logiciel de sauvegarde gratuit permettant de faire des sauvegardes.

<img width="967" height="625" alt="image" src="https://github.com/user-attachments/assets/18383bce-bebd-4fd1-ad7d-b56544862e7d" />

_____

🟢 **WordPress dans l'App Store de CasaOS :**
WordPress est un logiciel permettant de créer et gérer différents types de sites Web.

<img width="1283" height="683" alt="image" src="https://github.com/user-attachments/assets/45cec863-b12b-465f-8542-07b1e6634c89" />

