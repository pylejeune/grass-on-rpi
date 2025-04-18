# GRASS Token et DePIN : Installation sur Raspberry Pi

GRASS Network exploite le concept de DePIN (Decentralized Physical Infrastructure Network) pour miner des tokens.

## Mise à jour du système
```sh
sudo apt update && sudo apt upgrade -y
```

## Installer Docker (souvent requis pour DePIN nodes)
```sh
sudo apt install docker.io -y
```

## Éviter tout problème et exécuter Docker sans sudo
```sh
sudo usermod -aG docker $USER
newgrp docker
```

## Ajouter votre login et mot de passe en variable d'environnement
```sh
export GRASS_USER=username
export GRASS_PASS=password
```

## Démarrage du container Grass Node

[Référentiel GitHub](https://github.com/MRColorR/get-grass)
```sh
docker run -d --name grass -h my_device -e GRASS_USER=your_email -e GRASS_PASS=your_password mrcolorrain/grass
```

## Vérifier que le container est bien démarré
```sh
docker ps
```

## Suivre les logs du node Grass
```sh
docker logs -f grass
```

## Authentification sur l'interface
L'exécution attend que l'on se connecte avec notre login et mot de passe.
```sh
curl -X POST "https://app.getgrass.io/" \
     -d "username=${GRASS_USER}&password=${GRASS_PASS}" \
     -c cookies.txt
```

## Redémarrage en cas de crash du serveur

Créer ce script et l'appeler depuis un service

```sh
#!/bin/bash

export GRASS_USER=###YOUR_EMAIL###
export GRASS_PASS=###YOUR_PASSWORD###
docker run -d --rm --name grass -h my_device -e GRASS_USER=$GRASS_USER -e GRASS_PASS=$GRASS_PASS mrcolorrain/grass
curl -X POST "https://app.getgrass.io/" \
     -d "username=${GRASS_USER}&password=${GRASS_PASS}" \
     -c cookies.txt

while docker ps | grep -q grass; do
    sleep 1
done
```

## Finalisation
Une fois logué, le node Grass est en route et votre connexion est partagée. Vous commencez alors à générer des points qui seront transformés en tokens en fin de phase.
