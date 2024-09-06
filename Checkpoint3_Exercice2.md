# Partie 1 : Gestion des utilisateurs

### Q.2.1.1 :

Je crée un utilisateur "jerome" 

![ajout utilisateur](https://github.com/user-attachments/assets/268a6f05-67f5-46d4-8b7e-fb61639b8beb)

### Q.2.1.2

J'integre ce compte au groupe sudoers pour qu'il puisse configurer completement ma machine Linux 

Je me connecte avec le super-utilisateur "root" pour pouvoir l'integrer au groupe avec la commande 

su -

Le mot de passe du compte "root" m'est demandé 

![ajout groupe sudo ](https://github.com/user-attachments/assets/a8bf5dfb-040a-48e2-a7b2-848e028b147c)


# Partie 2 : Configuration de SSH

### Q.2.2.1 

Je vais dans le fichier de configuration  SSH 

 nano /etc/ssh/sshd_config

je vois que le fichier n'est autorisé qu'en ecriture alors je change ses droits avec la commande 

chmod 700 /etc/ssh/sshd_config

je retape la ligne de commande

nano /etc/ssh/sshd_config

je cherche dans le fichier la ligne 

#PermitRootLogin yes

et je la remplace par  #PermitRootLogin no

![no-SSH-root](https://github.com/user-attachments/assets/d8303b65-5421-494b-8541-4ecce709b04e)

### Q.2.2.2 
Je retourne dans le fichier de configuration SSH 

nano /etc/ssh/sshd_config

Je cherche la ligne AllowUer mais elle n'y est pas alors je la rajoute en ajoutant a la fin le'utilisateur que je veux autoriser a se connecter en SSH 

AllowUsers jerome

![autorisation SSH](https://github.com/user-attachments/assets/9f49d59d-6d3f-471e-99bf-5da6d3c69770)

### Q.2.2.3 

Il faut generer une clé SSH sur la machine distante avec laquelle vous voulez vous connecter si elle n'en dispose pas 

Entrez cette commande 

ssh-keygen -t rsa -b 4096

et completez les differentes phases de creation de clé  ou laissez par defaut les dossiers de configuration 

![generation de la clé](https://github.com/user-attachments/assets/5764801b-2733-4717-8c80-c1ee411982db)

Une fois la clé SSH créée ,copiez la sur la machine linux que vous voulez joindre (celle avec les restrictions precedement realisées)

ssh-copy-id jerome@server_ip_or_hostname (remplacez ip or hostname par les valeurs correspondantes)


 ### Q.2.2.3  

Retournez dans le fichier de configuration SSH 

sudo nano /etc/ssh/sshd_config

Cherchez la ligne  #PasswordAuthentication yes

Decommentez la et remplacez le "Yes" par "no"

![desactivation mot de passe](https://github.com/user-attachments/assets/785f0e18-818c-419c-8308-c1b846c4d322)

Cela a pour effet de ne pas autoriser l'authentification par mot de passe ,celle ci se fera  par clé precedement créée


# Partie 3 : Analyse du stockage

### Q.2.3.1

![fichiers montés](https://github.com/user-attachments/assets/cf24d3e3-fda9-437a-afd5-1efbad76a773)


### Q.2.3.2

![type fichiers montés](https://github.com/user-attachments/assets/b644f28b-5dd6-4129-93e1-a5fb38078621)

### Q.2.3.3

J'ajoute un nouveau disque de 8Go à ma machine dans la configuration de virtualbox 

je verifie l'etat du RAID avec cette commande

 mdadm --detail /dev/md0

md0 etant le nom de ma partition du RAID  sur mon disque 1 

je partitionne le nouveau disque et copie les partitions du premier disque sur  le nouveau disque  grace a la commande 

 sfdisk -d /dev/sda | sudo sfdisk /dev/sdb

j'ajoute le nouveau disque au volume RAID avec cette commande 

 mdadm --manage /dev/md0 --add /dev/sdb

je peux verifier l'avancement de la reconstruction du RAID  avec cette commande 

watch cat /proc/mdstat

il est prefereable de mettre a jour le fichier de configuration pour qu'il soit persistant apres un redemarrage 

sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf

Inclure la nouvelle configuration du RAID 

 update-initramfs -u

Verification de l'etat du RAID 

![etat du raid](https://github.com/user-attachments/assets/6442e93d-2db1-4b16-9c8f-4953b2a60c56)

### Q.2.3.4

je convertis mon nouveau disque en volume "physique"

 pvcreate /dev/sdc

je crée un groupe de volume 

vgcreate vg_sauvegardes /dev/sdc

Creer un volume logique

 lvcreate -L 1.5G -n lv_sauvegardes vg_sauvegardes (je n'ai pas pu creer un volume de 2Go car j'avais ajouter qu'un disque de 2Go a ma virtualbox et forcement il me disait que mon disque est trop petit )

je formate mon volume 

sudo mkfs.ext4 /dev/vg_sauvegardes/lv_sauvegardes

je crée un repertoire ou je vais monter le volume 

 mkdir -p /var/lib/bareos/storage

![creation volume bareos](https://github.com/user-attachments/assets/05084844-7c94-40d4-8409-cfae2bc5bb96)

configuration montage automatique et le tester 

j'ajoute une ligne dans le fichier /etc/fstab 

![ajout ligne fstab bareos](https://github.com/user-attachments/assets/d8bfbc83-8e2d-4d57-875a-9fadd3e7d45b)

![verif montage bareos](https://github.com/user-attachments/assets/2c1f8f2f-0772-4f3b-9a39-85f05ac2c3d3)

### Q.2.3.5

 ![espace libre](https://github.com/user-attachments/assets/0bceafe5-4ebb-42a7-9506-6a0b504ea2fd)

# Partie 4 : Sauvegardes

### Q.2.4.1

Bareos-Dir est ,si je puis dire ,le chef d'orchestre de Bareos 
Il  verifie les opérations de sauvegarde, de restauration, et de vérification.
Il gere la configuration des clients ,des taches de sauvegardes
Il envoie envoie des commandes aux autres composants, notamment au bareos-fd et au bareos-sd.

Bareos-sd
IL est responsable de l'enregistrement des données sauvegardées sur les supports de stockage

Bareos-fd

Il collecte les fichiers et données à sauvegarder sur le système de fichiers local et les envoie au bareos-sd suivant les instructions du bareos-dir

# Partie 5 : Filtrage et analyse réseau

### Q.2.5.1

![regles appliquées](https://github.com/user-attachments/assets/d55ef27c-6f3e-43b7-aa97-f915b29c9320)

### Q.2.5.2

les types de communication autorisées sont:

Tcp port 22
Icmp 
Icmp Ipv6

### Q.2.5.3

ct state invalid

### Q.2.5.4

#Ouvrir le port 9101 pour le Bareos Director (communication entrante)
sudo nft add rule inet filter input ip saddr (IP) tcp dport 9101 accept

#Ouvrir le port 9102 pour le Bareos File Daemon (communication entrante sur les clients)
sudo nft add rule inet filter input ip saddr (IP) tcp dport 9102 accept

#Ouvrir le port 9103 pour le Bareos Storage Daemon (communication entrante)
sudo nft add rule inet filter input ip saddr (IP) tcp dport 9103 accept

#Autoriser le trafic sortant vers les clients sur les ports 9101-9103
sudo nft add rule inet filter output ip daddr (IP) tcp sport 9101-9103 accept

# Partie 6 : Analyse de logs

### Q.2.6.1

![refus de co](https://github.com/user-attachments/assets/369034fd-21b9-4f9a-8bf3-53d5c74a7f74)

Adresse IP de la machine qui  a fait les tentatives :

# 10.0.0.199
















































