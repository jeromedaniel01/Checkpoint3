# Partie 1 : Gestion des utilisateurs

### Q.1.1.1

Je commence par creer  l'utilisateur Lionel Lemarchand en copiant le profil de Kelly Rhameur 

![creation lionel lemarchand](https://github.com/user-attachments/assets/fc1ccee3-0146-4cb9-8598-81b1f08c51f1)

### Q.1.1.2

Puis je crée une OU "DeactivedUsers"

![creation OU](https://github.com/user-attachments/assets/43d0323b-a654-493e-9106-03bc843c2ba4)

Apres je deplace le profil de Kelly Rhameur dans l'OU deactivedUsers

![move kelly](https://github.com/user-attachments/assets/9ad5729a-2bec-44cd-b08b-40baacf2b0bd)

### Q.1.1.3

Suppression de Kelly Rhameur de son ancienne OU 

![supp groupe kelly](https://github.com/user-attachments/assets/53c98a63-e90c-4be3-b324-cc025be2d82a)

en cliquant sur "remove"

### Q.1.1.4

![Archive Kelly](https://github.com/user-attachments/assets/7f76b232-69cc-400b-b9e1-b5d5181d4097)

![nouv dossier Lionel](https://github.com/user-attachments/assets/67f31951-79a9-48fc-9052-244f9330bb0f)

# Partie 2 : Restriction utilisateurs

### Q.1.2.1

cliquez sur l'onglet "Logon Hours" dans les proprietes de l'onglet Account

![horaire gabriel](https://github.com/user-attachments/assets/135de6bf-47b0-42ca-8bf2-3da8ebe4c832)

### Q.1.2.2

cliquez sur l'onglet "Log On To" dans les proprietes de l'onglet Account

![ordi gabriel](https://github.com/user-attachments/assets/e8c32e4c-6411-4620-9647-77e528491407)

### Q.1.2.3

Allez dans la creation de gpo avec touche windows + R  et ecrire gpcmc.msc

cette commande ouvre l'editeur de GPO 


![creation GPO MDP](https://github.com/user-attachments/assets/533a9aa2-4c12-4421-8856-b9a6d6285027)

Mot de passe de 8 caracteres minimum

![caractere Mini MDP](https://github.com/user-attachments/assets/7f1e6381-5a4f-4b7c-8076-708147b10a40)


lier la GPO a l'OU LabUser

![lier GPO](https://github.com/user-attachments/assets/ebd55da7-d551-4926-b733-c74b44a46b87)

# Partie 3 : Lecteurs réseaux

Allez dans la creation de gpo avec touche windows + R  et ecrire gpcmc.msc

cette commande ouvre l'editeur de GPO 

![gpo lecteur mappé](https://github.com/user-attachments/assets/40e8cf79-9be8-40aa-9f7e-4ed74fbceef2)

![GPO 2lecteurs](https://github.com/user-attachments/assets/fdab7556-5ace-43da-af0b-b020dc2c9908)

mes deux lecteurs sont dans la GPO 

je vais a present partager mes deux lecteurs avec le groupe domain computers (les clients)
je vais dans this pc et je clique droit sur le lecteur que je veux partager pour afficher ses propriétés
je clique sur l'onglet sharing 
je lui donne le nom DossiersCommuns pour le lecteur E et DossiersIndividuels pour le lecteurs F 
j'autorise seulement le groupe Domain Computer pour le partage 

![partage lecteur](https://github.com/user-attachments/assets/60343c12-4e02-4d30-8908-54407b866e7a)

![partage E](https://github.com/user-attachments/assets/6acd0ae2-8f80-4a8d-a402-2bd2a687634e)








































