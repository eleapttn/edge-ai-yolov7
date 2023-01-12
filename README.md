# Edge AI : allez viens, on embarque notre intelligence artificielle !


## Présentation



## CLI ovhai

1. Login: `ovhai login -u <username> -p <password>`
2. Check the notebooks in **RUNNING** status: `ovhai notebook list --states RUNNING`
3. Check information about **edge-ai-yolov7-notebook**: `ovhai notebook get <notebook-id>`
4. *Access to the notebook with the URL* > **AI Notebooks**
5. Check the availability of the model **yolov7-tiny.pt** in the dedicated Object Storage container: `ovhai data list GRA edge-ai-yolov7-model`
6. Download the model locally in `/tmp`: `cd /tmp` and then `ovhai data download GRA edge-ai-yolov7-model`
7. *Test the YOLOv7 tiny model on the Raspberry Pi4* > **Raspberry Pi 4**

## AI Notebooks

Once you are inside the AI Notebook, play the different steps. 

*The training could take several hours...*

When the training is finished, save and export the model inside the dedicated Object Storage container.

Turn to **CLI ovhai - step 5**.

## Raspberry Pi 4

Go to `/tmp` directory and clone YOLOv7 repository: `git clone https://github.com/WongKinYiu/yolov7.git`

Go to `/yolov7` directory: `cd /yolov7`

Install YOLOv7 dependences via the pip command: `pip3 install -r requirements.txt`

Test your model on existing images with the following commmand: `python3 detect.py --weights '/tmp/yolov7-tiny.pt` --conf 0.25 --img-size 640 --source inference/images/keyboard.jpg`


Try it for real-time detection:

Finally, take a picture, save it and use the model in order to detect objects:
