# Jetracer

Le Jetracer est un robot conçu par Waveshare basé sur une carte NVIDIA Jetson Nano. Il permet principalement de se familiariser avec la conduite autnonome basé sur les réseaux de neurone.

## Accès à l'interface du robot

Il existe plusieurs méthode pour se connecter au robot et développer des algorithmes de conduites autonomes.

### Accès physique

La première méthode est la méthode conseillée pour la phase de développement. Pour cela, il faut disposer d'un moniteur, d'une souris et d'un clavier. Brancher le clavier et la souris sur les port USB de la NVIDIA Jetson Nano. Ensuite brancher le moniteur sur le port HDMI ou le port Display port. Il est également conseillé de brancher la NVIDIA Jetson Nano a une source de tension différente des batteries durant la phase de développement.

### Accès à distance

Il est également possible d'accès à l'interface du robot à distance à partir du serveur Jupyter disponible sur la NVIDIA Jetson Nano. Pour cela, il faut que le robot et l'ordinateur à partir duquel on tente accéder au robot soient sur le même réseau local. Lorsque le robot est connecté sur un réseau, l'addresse IP du robot est alors affiché sur l'écran du robot, il faut alors recopier cette adresse IP sur un navigateur web suivi du numéro de port 8888. Par exemple, pendant le stage, l'addresse IP du Jetracer était `192.168.0.104`, on écrit donc sur un navigateur web la ligne suivante :

```bash
http://192.168.0.104:8888/
``` 
## Calibration de la caméra 

Il se peut qu'à la première utilisation, la caméra du robot ait une teinte rougeâtre. Pour corriger ce problème, il faut appliquer un correctif téléchargeable [ici.](https://files.waveshare.com/upload/e/eb/Camera_overrides.tar.gz)

Il faut ensuite lance les lignes de commandes suivantes : 
```bash
tar zxvf Camera_overrides.tar.gz 
sudo cp camera_overrides.isp /var/nvidia/nvcam/settings/
sudo chmod 664 /var/nvidia/nvcam/settings/camera_overrides.isp
sudo chown root:root /var/nvidia/nvcam/settings/camera_overrides.isp
```

## Prise en main du robot

Il est vivement conseillé de se familisariser avec le robot avant de commencer à travailler sérieusement dessus. Pour cela, il y a deux codes (réalisé par NVIDIA) dans le répertoire `code basique`, le premier `basic_motion.ipynb` permet de se familiariser avec le système de conduite du Jetracer et le code `teleoperation.ipynb` permet de piloter le robot à distance avec une manette. Ce code sera nécessaire durant les phases de collecte de données.

## Suivi de trajectoire

Maintenant que vous savez utiliser le robot, on peut passer à l'algorithme de conduite autonome disponible dans le dossier `Road following`. Pour cela, il faut entraîner, optimiser puis inférérer un réseau de neurone.
Pour l'entrainement, il faut utiliser le code `interactive_regression.ipynb` qui permet de collecter les données, les annoter et de lancer l'entrainement.
Ensuite, pour l'étape d'opimisation et d'inférence, il faudra utiliser le code `road_following.ipynb`.
Une fois la premère inférence réussi, on pourra utiliser le code `RF_inference.ipynb` qui permet qui contient un code optimiser pour l'inférence basé sur le précédent.

## Evitement d'obstacle
