# Jetracer

Le Jetracer est un robot conçu par Waveshare basé sur une carte NVIDIA Jetson Nano. Il permet principalement de se familiariser avec la conduite autonome basée sur les réseaux de neurones.

## Accès à l'interface du robot

Il existe plusieurs méthodes pour se connecter au robot et développer des algorithmes de conduites autonomes.

### Accès physique

La première méthode est la méthode recommandée pour la phase de développement. Pour cela, il faut disposer d'un moniteur, d'une souris et d'un clavier. Branchez le clavier et la souris sur les ports USB de la NVIDIA Jetson Nano. Ensuite branchez le moniteur sur le port HDMI ou le port Display port. Il est également conseillé de brancher la NVIDIA Jetson Nano à une source de tension différente des batteries durant la phase de développement.

### Accès à distance

Il est également possible d'accéder à l'interface du robot à distance à partir du serveur Jupyter disponible sur la NVIDIA Jetson Nano. Pour cela, il faut que le robot et l'ordinateur à partir duquel on tente accéder au robot soient sur le même réseau local. Lorsque le robot est connecté sur un réseau, l'adresse IP du robot est alors affiché sur l'écran du robot, il faut alors recopier cette adresse IP sur un navigateur web suivi du numéro de port 8888. Par exemple l'adresse IP du Jetracer utilisé était `192.168.0.104`, on écrit donc sur un navigateur web la ligne suivante :

```bash
http://192.168.0.104:8888/
``` 
## Calibration de la caméra 

Il se peut qu'à la première utilisation, la caméra du robot ait une teinte rougeâtre. Pour corriger ce problème, il faut appliquer un correctif téléchargeable [ici.](https://files.waveshare.com/upload/e/eb/Camera_overrides.tar.gz)

Il faut ensuite lance les lignes de commande suivantes : 
```bash
tar zxvf Camera_overrides.tar.gz 
sudo cp camera_overrides.isp /var/nvidia/nvcam/settings/
sudo chmod 664 /var/nvidia/nvcam/settings/camera_overrides.isp
sudo chown root:root /var/nvidia/nvcam/settings/camera_overrides.isp
```

## Prise en main du robot

Il est vivement conseillé de se familisariser avec le robot avant de commencer à travailler sérieusement dessus. Pour cela, il y a deux codes (réalisé par NVIDIA) dans le répertoire `code basique`, le premier `basic_motion.ipynb` permet de se familiariser avec le système de conduite du Jetracer et le code `teleoperation.ipynb` permet de piloter le robot à distance avec une manette. Ce code sera nécessaire durant les phases de collecte de données.

## Suivi de trajectoire

Maintenant que vous savez utiliser le robot, on peut passer à l'algorithme de conduite autonome disponible dans le dossier `Road following`. Pour cela, il faut entraîner, optimiser puis faire l'inférence du réseau de neurone.
Pour l'entrainement, il faut utiliser le code `interactive_regression.ipynb` qui permet de collecter les données, les annoter et de lancer l'entrainement.
Ensuite, pour l'étape d'optimisation et d'inférence, il faudra utiliser le code `road_following.ipynb`.
Une fois la première inférence réussie, on pourra utiliser le code `RF_inference.ipynb` qui permet qui contient un code optimisé pour l'inférence basé sur le précédent.

## Evitement d'obstacle

Les codes utiles pour l'évitement d'obstacle se trouvent dans le répertoire `Evitement obstacle`. Tout comme le suivi de trajectoire, il faut commencer par créer une base de données, pour cela, il faut utiliser le code `Data_Collection.ipynb`. 
Une fois les données collectées, on peut entrainer le modèle à l'aide du code `Obstacle_training.ipynb`. Ensuite on peut passer à l'optimisation avec le code `live_demo_resnet18_build_trt.ipynb`. Et enfin, une fois le moteur d'inférence prêt, on peut lancer une inférence à partir du code `live_demo_resnet18_trt.ipynb`. 
À noter que la sortie du modèle de détection d'obstacle est une probabilité comprise entre 0 et 1, il est normalement proche de 0.95 en cas de détection d'obstacle et proche de 0 s'il n'y a pas 'obstacle. Si ce n'est pas le cas, cela signifie que la base de données n'est pas complète. Il faut collecter des images de l'obstacle sur des fonds différents pour pas que certains motifs du fond ne soient pas pris comme obstacle.

## Script combiné

Une fois les deux modèles prêts, on peut alors passer à l'étape finale. Cet algorithme va permettre de suivre une trajectoire par défaut puis en cas de détection d'obstacle, il va l'éviter puis reprendre la trajectoire initiale. Le code correspondant se nomme `DEMO1.ipynb` et se trouve `Script combiné` 

## Réseau

Dans le but du projet V2X, il est intéressant de rendre la conduite connecté. Pour cela, les codes nécessaires sont dans le répertoire `Réseau`. Le code permettant d'envoyer le flux de la caméra sur un serveur distant se nomme `Video_flask.ipynb`

## Crédit

Une grande partie des codes développés dans ce projet sont basés sur ceux de NVIDIA. C'est notamment le cas des codes `basic_motion.ipynb` `teleoperation.ipynb` `interactive_regression.ipynb` `road_following.ipynb` `Data_Collection.ipynb` `Obstacle_training.ipynb` `live_demo_resnet18_build_trt.ipynb` `live_demo_resnet18_trt.ipynb` et sont disponibles sur le [Github de NVIDIA](https://github.com/NVIDIA-AI-IOT/jetracer)
