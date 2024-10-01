## Getting Started with DifAda



### Installation

The codebases are main built on top of [DiffusionDet](https://github.com/ShoufaChen/DiffusionDet).Thanks very much.

#### Requirements
[requirement](environment.yaml) is exported from our virtual environment, which is for reference only.
We suggest following [detectron2](https://github.com/facebookresearch/detectron2/blob/main/GETTING_STARTED.md).
- Linux or macOS with Python ≥ 3.6
- PyTorch ≥ 1.9.0 and [torchvision](https://github.com/pytorch/vision/) that matches the PyTorch installation.
  You can install them together at [pytorch.org](https://pytorch.org) to make sure of this
- OpenCV is optional and needed by demo and visualization

#### Steps
1. Install Detectron2 following https://github.com/facebookresearch/detectron2/blob/main/INSTALL.md#installation.

2. Prepare datasets
```
COCO:
mkdir -p datasets/coco
ln -s /path_to_coco_dataset/annotations datasets/coco/annotations
ln -s /path_to_coco_dataset/train2017 datasets/coco/train2017
ln -s /path_to_coco_dataset/val2017 datasets/coco/val2017

RUOD:
Please download them to `DifAda_ROOT/datasets/RUOD`
```

3. Prepare pretrain models

DifAda uses backbones including ResNet-50, ResNet-101. The pretrained ResNet-50 model can be
downloaded automatically by Detectron2. We also provide pretrained
[ResNet-101](https://github.com/ShoufaChen/DiffusionDet/releases/download/v0.1/torchvision-R-101.pkl)
Detectron2. Please download them to `DifAda_ROOT/models/` before training:

```bash
mkdir models
cd models
# ResNet-101
wget https://github.com/ShoufaChen/DiffusionDet/releases/download/v0.1/torchvision-R-101.pkl
cd ..
```

Thanks for model conversion scripts of [ResNet-101](https://github.com/PeizeSun/SparseR-CNN/blob/main/tools/convert-torchvision-to-d2.py)


4. Train DifAda
```
python train_net.py
```

5. Evaluate DifAda
```
python train_net.py --eval-only MODEL.WEIGHTS path/to/model.pth
```

* Evaluate with 4 refinement steps and 6 iterations by setting `MODEL.DiffusionDet.SAMPLE_STEP 4`,`MODEL.DiffusionDet.NUM_HEADS 6`.


We provide the model [COCO result](coming soon) of [Difada-300boxes](configs/difada.coco.res50.yaml).


### Inference Demo with Pre-trained Models
These follow [DiffusionDet](https://github.com/ShoufaChen/DiffusionDet).
They provide a command line tool to run a simple demo following [Detectron2](https://github.com/facebookresearch/detectron2/tree/main/demo#detectron2-demo).

Download demo.py from[DiffusionDet](https://github.com/ShoufaChen/DiffusionDet).

```bash
python demo.py --config-file configs/diffdet.coco.res50.yaml \
    --input image.jpg --opts MODEL.WEIGHTS diffdet_coco_res50.pth
```

They need to specify `MODEL.WEIGHTS` to a model from model zoo for evaluation.
This command will run the inference and show visualizations in an OpenCV window.

For details of the command line arguments, see `demo.py -h` or look at its source code
to understand its behavior. Some common arguments are:
* To run __on your webcam__, replace `--input files` with `--webcam`.
* To run __on a video__, replace `--input files` with `--video-input video.mp4`.
* To run __on cpu__, add `MODEL.DEVICE cpu` after `--opts`.
* To save outputs to a directory (for images) or a file (for webcam or video), use `--output`.
