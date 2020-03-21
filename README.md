Currently the repo contains only Darknet-based YOLO models converted to TensorFlow using [Darkflow](https://github.com/thtrieu/darkflow). 

## Some of the already converted model files are linked below
|YOLO Model | Train Dataset | mAP | FPS | Frozen Model (.pb) | Metafile
|--|--|--|--|--|--|
|Darknet19 (448x448) | VOC-1000 | 76.4 | - | [pb](https://drive.google.com/file/d/1-JcltriqDR8UYFCvjdiMi5EZHSkhEEO4/view?usp=sharing) | [meta](https://drive.google.com/file/d/1-KP9mzUEIVjDGJPGM1P8JcWjd6CUe_LW/view?usp=sharing)|
|v2-tiny | COCO | - | 200 | [pb](https://drive.google.com/file/d/1-PKlksWFeOYD96vDik4KCoOoEJKLeVhn/view?usp=sharing) | [meta](https://drive.google.com/file/d/1-SFr-R1SkxC7ZP-56-BBt-9ieab8WOgn/view?usp=sharing)|
|v2-tiny | VOC | 57.1 |207 | failed | failed|


## The conversion for the YOLO v2-tiny (COCO dataset) is done as:

1) Install Dependencies
```
!apt-get update
!pip3 install numpy
!apt-get install python-opencv -y
!pip install cython
```

2) Clone and install Darkflow
```
# Clone the repository
!git clone https://github.com/thtrieu/darkflow
# Change into the darkflow dir and install darkflow with pip
%cd darkflow
!python setup.py build_ext --inplace
```

3) Fix assertion issue by modifying the `loader.py` 
_Note: may not always be needed but it won't cause any issue_
```
#Change directory to darkflow
%cd /content/darkflow
!sed -i "s/self.offset = 16/self.offset = 20/g"  ./darkflow/utils/loader.py
```

4) Download the model files (`cfg` file + `weights` file)
```
!wget -O "cfg/yolo-v2-tiny-coco.cfg" "https://raw.githubusercontent.com/pjreddie/darknet/master/cfg/yolov2-tiny.cfg"
!wget -O "weights/yolo-v2-tiny-coco.weights" "https://pjreddie.com/media/files/yolov2-tiny.weights"
```

5) Convert using `flow` and generate frozen graph (`.pb`)
_Note: We keep cfg files in the darkflow/cfg directory and the weights in the darkflow/weights directory_
```
#syntax: $ !./flow --model path_to_cfg_file --load path_to_weights --savepb
!./flow --model cfg/yolo-v2-tiny-coco.cfg --load weights/yolo-v2-tiny-coco.weights --savepb
```

## Done ! You should be able to find your converted frozen graph in the `darkflow/built_graph` directory
