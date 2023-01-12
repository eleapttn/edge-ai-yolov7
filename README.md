# Edge AI : allez viens, on embarque notre intelligence artificielle !

## Introduction

Avez-vous déjà entendu parler du terme "AI on edge" ? Il s'agit du déploiement d'applications d'IA sur des appareils situés dans le monde physique. Les avantages ? Moins de latence, plus de sécurité, d'efficacité et surtout de la proximité ! Aujourd'hui, il devient donc de plus en plus important de pouvoir déployer des modèles d'IA capables d'inférer en temps réel.

La vision par ordinateur est particulièrement concernée de par sa progression rapide et son utilisation dans de nombreux domaines : automobile, médical, commerce, … Elle regroupe de nombreuses techniques comme la classification d'images, la segmentation d'images ou encore la détection d'objets.

Cette dernière permet d'identifier et de localiser les différents objets sur une image ou sur une vidéo. Un célèbre algorithme de détection d'objets, connu pour son fonctionnement rapide, se nomme YOLOv7.

Dans ce talk, nous verrons comment déployer un modèle YOLOv7 pour la détection d'objets sur une carte Raspberry Pi 4.

Pour cela, nous nous intéresserons à l'entraînement et au test d'un modèle YOLOv7 au sein d'un Notebook Jupyter. Nous convertirons ensuite notre modèle pour pouvoir le déployer et faire de l'inférence sur Raspberry Pi. La finalité ? Un outil de détection d'objets en temps réel à portée de main.

Alors, on embarque ?


## Requirements

- An OVHcloud Public Cloud Project if you want to test it with OVHcloud AI Tools
- A Raspberry Pi 4

## Instructions

### Step 1 - Login directly in the CLI with your credentials

`ovhai login -u <username> -p <password>`

### Step 2 - Check the **RUNNING** notebooks and copy your notebook id

`ovhai notebook list --states RUNNING`

### Step 3 - Check information about **edge-ai-yolov7-notebook**

`ovhai notebook get <notebook-id>`

### Step 4 - Access to the notebook with the URL

Once you are inside the AI Notebook, play the different steps. 

*The training could take several hours...*

When the training is finished, save and export the model inside the dedicated Object Storage container.

### Step 5 - Check the availability of the model `yolov7-tiny.pt` in the dedicated Object Storage container

`ovhai data list GRA edge-ai-yolov7-model`

### Step 6 - Clone YOLOv7 repository

1. Go to `/tmp` directory: : `cd /tmp`
2. Clone YOLOv7 repository: `git clone https://github.com/WongKinYiu/yolov7.git`
3. Access to `/yolov7` directory: `cd /yolov7`

### Step 7 - Install YOLOv7 dependences via the pip command

`pip3 install -r requirements.txt`

### Step 8 - Download the model from the Cloud

Once you are in the `/yolov7` directory, launch the following command: `ovhai data download GRA edge-ai-yolov7-model`

### Step 9 - Test the YOLOv7 tiny model on the Raspberry Pi 4

Here, three tests are done:

#### Test n°1 - Run inference on existing images

Test your model on existing images with the following commmand: 
`python3 detect.py --weights yolov7-tiny.pt --conf 0.25 --img-size 640 --source inference/images/keyboard.jpg`

#### Test n°2 - Play with the real-time detection

Try it for real-time detection: 
`python3 detect.py --weights yolov7-tiny.pt --conf 0.25 --img-size 640 --source 0`

#### Test n°3 - Take new images and test the model

Create the new directories for new images and labels: 
- `mkdir new-data`
- `mkdir new-data/images`
- `mkdir new-data/labels`

Take pictures (with **Cheese** for example), save them and use the model in order to detect objects.

Launch the following command: 
`python3 detect.py --weights yolov7-tiny.pt --conf 0.25 --img-size 640 --source new-images/ --save-txt`

### Step 10 - Extract the new images and labels

Copy the new labels into the dedicated directory: `cp -a runs/detect/exp/labels/. new-data/labels/`

Content of the `new-data` directory:

```
.
├── images
│   └── test1.jpg
└── labels
    └── test1.txt

2 directories, 2 files
```

### Step 11 - Send the new processed data in the Cloud

Push the new data to the Object Storage container: `ovhai data upload GRA edge-ai-yolov7-data new-data/`

Synchronize the Object Storage and the notebook (pull the data): `ovhai notebook pull-data <notebook-id>`

### Step 12 - Check the new data availability in the notebook

New data are available? You are ready to train again your YOLOv7 model!
