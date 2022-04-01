I really hope you see this **README.md** firstly, thank you. 

This is a tutorial you can follow to train **yolov5-5.0 on crowdhuman dataset**. Because I'm also a newbie, I just  write this and share what I've done. I'd like you also refer to [the original yolov5](https://github.com/ultralytics/yolov5).If you think this tutorial is useful, I wish you can star it.



## Requirements

Python 3.8 or later with all [requirements.txt](https://github.com/ultralytics/yolov5/blob/master/requirements.txt) dependencies installed, including `torch>=1.7`. To install run:

```bash
$ pip install -r requirements.txt
```

Next, plz download models(.pt file) here and you can create new folder `/weights` to put  these:

<https://github.com/ultralytics/yolov5/releases/download/v5.0/yolov5m.pt>

**Attention:** **I employ the yolov5-5.0 version.**If you use the newest release, just change **'v5.0'** above to **'6.0'** (for example). Also, you can change **'yolov5m.pt'** to the **.pt file** you want.

 

## Dectect

`detect.py` runs inference on a variety of sources, downloading [models](https://github.com/ultralytics/yolov5/tree/master/models) automatically from the latest YOLOv5 [release](https://github.com/ultralytics/yolov5/releases) and saving results to `runs/detect`. (Just use this and try what you have employed can run.)

```
python detect.py --source 0  # webcam
                          img.jpg  # image
                          vid.mp4  # video
                          path/  # directory
                          path/*.jpg  # glob
                          'https://youtu.be/Zgi9g1ksQHc'  # YouTube
                          'rtsp://example.com/media.mp4'  # RTSP, RTMP, HTTP stream
```



## Convert  Crowdhuman  Format to YOLOv5 Format

1)  Tools: [Here](https://github.com/shaoshengsong/YOLOv5-Tools)

CrowdHuman2YOLO Structure：

```
data
├── crowdhuman.names
├── crowdhuman-template.data
├── gen_txts.py
├── prepare_data.sh
├── raw
└── verify_txts.py
```

2)  CrowdHuman Dataset: [Here](https://www.crowdhuman.org/)

CrowdHuman Dataset Structure：

```
CrowdHuman_train01.zip
CrowdHuman_train02.zip
CrowdHuman_train03.zip
CrowdHuman_val.zip
annotation_train.odgt
annotation_val.odgt
```

**Put all files above to data/raw <u>without unzip</u>.**

run

```
./prepare_data.sh 608x608
```

result

```
(pytorch1.7) /pytorch1.7/yolov4_crowdhuman/data$ ./prepare_data.sh 608x608
** Install requirements
** Download dataset files
** Unzip dataset files
Archive:  CrowdHuman_train01.zip
  inflating: Images/273271,1017c000ac1360b7.jpg  
  inflating: Images/273271,10355000e3a458a6.jpg  
  inflating: Images/273271,1039400091556057.jpg  
......

Processing ID: 273275,10b78d0006d7d7b9c
Processing ID: 284193,2e02d0003abd1bb1

** for yolov5-608x608, resized bbox width/height clusters are: (11.77, 22.62) (24.71, 61.38) (37.78, 117.48) (54.99, 187.92) (70.04, 270.74) (90.15, 371.35) (126.91, 492.11) (195.65, 316.45) (279.13, 521.00)
```

Now, we have convert convert the Crowdhuman dataset format to YOLOv5 format successfully.



## Train on Crowdhuman Dataset

Here, I take `yolov5m` for example.

First,  add an `.yaml` config file in `yolov5/data`：

```
# crowdhuman.yaml
 
train: "./data/crowdhuman-608x608/train.txt" 
val: "./data/crowdhuman-608x608/test.txt"
#path to your data train.txt or test.txt
#(also both them are in the same folder,that dosen't matter)

nc: 2
names: ["head", "person"]
#you can also change this classess
```

Next, enter the `/yolov5`, run the scipt(The model size profile selected in models should be the same as the pre-trained model in /`weights`)

```
python train.py --data data/crowdhuman.yaml --cfg models/yolov5m.yaml  --weights weights/yolov5m.pt --device 0
```

The result can be shown later, which is up to your device. If you have any issues, just question and I'll try my best to answer you.