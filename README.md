# Comments by Hosung Joo

My project aims to run AlphaPose in miniconda environment.

Download this frozen (and modified) project(~125MiB) on your <i>desired directory</i> by:
```shell
git clone https://github.com/hsjoo-lghtsnd/AlphaPose
```

Pre-trained models are very heavy (>1GB), so be prepared for the data size.

## Installation

#### NOTE

Note that this program requires 'libyaml-dev' package installed. You may need <i>sudo</i> operation.

```shell
sudo apt-get install libyaml-dev
```

### miniconda installation
Install your conda environment by:

```shell
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```

You may reopen your terminal. You will see (base) in your terminal line. You can turn off the auto conda startup on terminal by typing:

```shell
conda config --set auto_activate_base false
```

### pytorch installation
Make sure you have created a new environment for this project as:
```shell
conda create --name AlphaPose
conda activate AlphaPose
```
#### NOTE
You can remove the environment by:
```shell
conda env remove -n AlphaPose
```

Make sure that your environment is activated. Install your pytorch and other requirements.
```shell
conda install pyyaml==5.2
conda install scipy
conda install pytorch torchvision cudatoolkit=11.3 -c pytorch
python -m pip install cython
```

#### NOTE

Note that running below requires your machine path directory.

![conda-cuda](https://user-images.githubusercontent.com/46191084/180616057-f90c45c1-d938-401f-bb32-112500614194.png)

Run the shell scripts below. This takes very long time. (With less than 10 minutes, it will be compiled by using <i>nvcc</i> cuda compiler.) You may see several warning signs, but as long as you don't actually see a error message, it is fine.

```shell
export PATH=/usr/local/cuda-11.3/bin/:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.3/lib64/:$LD_LIBRARY_PATH
cd AlphaPose
python setup.py build develop
```

## Downloading Pre-trained Models

Pre-trained models are very heavy. Although we can use wget for ease. (~1GB)

```shell
mkdir ./detector/yolo/data
mkdir ./detector/tracker/data
mkdir ./detector/yolox/data

wget -P ./detector/yolox/data/ https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.0/yolox_x.pth

```

However, they provide these files by GoogleDrive. [yolov3-spp.weights](https://drive.google.com/open?id=1D47msNOOiJKvPOXlnpyzdKA3k6E97NTC),
[MSCOCO FastPose](https://drive.google.com/open?id=1kQhnMRURFiy7NsdS8EFL-8vtqEXOgECn).

Put **yolov3-spp.weights** in <i>(AlphaPoseRoot)</i>/detector/**yolo**/data,
and **fast_res50_256x192.pth** in <i>(AlphaPoseRoot)</i>/detector/**tracker**/data.

You can run below:
```shell
python3 scripts/demo_inference.py --cfg configs/coco/resnet/256x192_res50_lr1e-3_1x.yaml --checkpoint pretrained_models/fast_res50_256x192.pth --indir examples/demo/ --save_img
```
Then, the result will appear in examples/demo/res. Visual representation will be included in examples/demo/res/vis. You can replace the files in examples/demo for more inference.

![1](https://user-images.githubusercontent.com/46191084/180618128-3e7b7610-1a09-41c1-a1e0-57d298d2bdd9.jpg)




#### NOTE
(The explanation below is copied from /docs/install.md)

### Models
1. Download the object detection model manually: **yolov3-spp.weights**([Google Drive](https://drive.google.com/open?id=1D47msNOOiJKvPOXlnpyzdKA3k6E97NTC) | [Baidu pan](https://pan.baidu.com/s/1Zb2REEIk8tcahDa8KacPNA)). Place it into `detector/yolo/data`.
2. ~~(Optional) If you want to use [YOLOX](https://github.com/Megvii-BaseDetection/YOLOX) as the detector, you can download the weights [here](https://github.com/Megvii-BaseDetection/YOLOX), and place them into `detector/yolox/data`. We recommend [yolox-l](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_l.pth) and [yolox-x](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_x.pth).~~<i>Done by wget shell scripts.</i>
3. Download our pose models. Place them into `pretrained_models`. All models and details are available in our [Model Zoo](./MODEL_ZOO.md).
4. For pose tracking, please refer to our [tracking docments](../trackers) for model download


- - -
- - -


# Original README.md

<div align="center">
    <img src="docs/logo.jpg", width="400">
</div>


## News!
- Jan 2022: [**v0.5.0** version](https://github.com/MVIG-SJTU/AlphaPose) of AlphaPose is released! Stronger whole body(face,hand,foot) keypoints! More models are availabel. Checkout [docs/MODEL_ZOO.md](docs/MODEL_ZOO.md)
- Aug 2020: [**v0.4.0** version](https://github.com/MVIG-SJTU/AlphaPose) of AlphaPose is released! Stronger tracking! Include whole body(face,hand,foot) keypoints! [Colab](https://colab.research.google.com/drive/1c7xb_7U61HmeJp55xjXs24hf1GUtHmPs?usp=sharing) now available.
- Dec 2019: [**v0.3.0** version](https://github.com/MVIG-SJTU/AlphaPose) of AlphaPose is released! Smaller model, higher accuracy!
- Apr 2019: [**MXNet** version](https://github.com/MVIG-SJTU/AlphaPose/tree/mxnet) of AlphaPose is released! It runs at **23 fps** on COCO validation set.
- Feb 2019: [CrowdPose](https://github.com/MVIG-SJTU/AlphaPose/docs/CrowdPose.md) is integrated into AlphaPose Now!
- Dec 2018: [General version](https://github.com/MVIG-SJTU/AlphaPose/trackers/PoseFlow) of PoseFlow is released! 3X Faster and support pose tracking results visualization!
- Sep 2018: [**v0.2.0** version](https://github.com/MVIG-SJTU/AlphaPose/tree/pytorch) of AlphaPose is released! It runs at **20 fps** on COCO validation set (4.6 people per image on average) and achieves 71 mAP!

## AlphaPose
[AlphaPose](http://www.mvig.org/research/alphapose.html) is an accurate multi-person pose estimator, which is the **first open-source system that achieves 70+ mAP (75 mAP) on COCO dataset and 80+ mAP (82.1 mAP) on MPII dataset.** 
To match poses that correspond to the same person across frames, we also provide an efficient online pose tracker called Pose Flow. It is the **first open-source online pose tracker that achieves both 60+ mAP (66.5 mAP) and 50+ MOTA (58.3 MOTA) on PoseTrack Challenge dataset.**

AlphaPose supports both Linux and **Windows!**

<div align="center">
    <img src="docs/alphapose_17.gif", width="400" alt><br>
    COCO 17 keypoints
</div>
<div align="center">
    <img src="docs/alphapose_26.gif", width="400" alt><br>
    <b><a href="https://github.com/Fang-Haoshu/Halpe-FullBody">Halpe 26 keypoints</a></b> + tracking
</div>
<div align="center">
    <img src="docs/alphapose_136.gif", width="400"alt><br>
    <b><a href="https://github.com/Fang-Haoshu/Halpe-FullBody">Halpe 136 keypoints</a></b> + tracking
</div>


## Results
### Pose Estimation
Results on COCO test-dev 2015:
<center>

| Method | AP @0.5:0.95 | AP @0.5 | AP @0.75 | AP medium | AP large |
|:-------|:-----:|:-------:|:-------:|:-------:|:-------:|
| OpenPose (CMU-Pose) | 61.8 | 84.9 | 67.5 | 57.1 | 68.2 |
| Detectron (Mask R-CNN) | 67.0 | 88.0 | 73.1 | 62.2 | 75.6 |
| **AlphaPose** | **73.3** | **89.2** | **79.1** | **69.0** | **78.6** |

</center>

Results on MPII full test set:
<center>

| Method | Head | Shoulder | Elbow | Wrist | Hip | Knee | Ankle | Ave |
|:-------|:-----:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
| OpenPose (CMU-Pose) | 91.2 | 87.6 | 77.7 | 66.8 | 75.4 | 68.9 | 61.7 | 75.6 |
| Newell & Deng | **92.1** | 89.3 | 78.9 | 69.8 | 76.2 | 71.6 | 64.7 | 77.5 |
| **AlphaPose** | 91.3 | **90.5** | **84.0** | **76.4** | **80.3** | **79.9** | **72.4** | **82.1** |

</center>

More results and models are available in the [docs/MODEL_ZOO.md](docs/MODEL_ZOO.md).

### Pose Tracking

<p align='center'>
    <img src="docs/posetrack.gif", width="360">
    <img src="docs/posetrack2.gif", width="344">
</p>

Please read [trackers/README.md](trackers/) for details.

### CrowdPose
<p align='center'>
    <img src="docs/crowdpose.gif", width="360">
</p>

Please read [docs/CrowdPose.md](docs/CrowdPose.md) for details.


## Installation
Please check out [docs/INSTALL.md](docs/INSTALL.md)

## Model Zoo
Please check out [docs/MODEL_ZOO.md](docs/MODEL_ZOO.md)

## Quick Start
- **Colab**: We provide a [colab example](https://colab.research.google.com/drive/1c7xb_7U61HmeJp55xjXs24hf1GUtHmPs?usp=sharing) for your quick start.

- **Inference**: Inference demo
``` bash
./scripts/inference.sh ${CONFIG} ${CHECKPOINT} ${VIDEO_NAME} # ${OUTPUT_DIR}, optional
```
For high level API, please refer to `./scripts/demo_api.py`. To enable tracking, please refer to [this page](./trackers).

- **Training**: Train from scratch
``` bash
./scripts/train.sh ${CONFIG} ${EXP_ID}
```

- **Validation**: Validate your model on MSCOCO val2017
``` bash
./scripts/validate.sh ${CONFIG} ${CHECKPOINT}
```

Examples:

Demo using `FastPose` model.
``` bash
./scripts/inference.sh configs/coco/resnet/256x192_res50_lr1e-3_1x.yaml pretrained_models/fast_res50_256x192.pth ${VIDEO_NAME}
#or
python scripts/demo_inference.py --cfg configs/coco/resnet/256x192_res50_lr1e-3_1x.yaml --checkpoint pretrained_models/fast_res50_256x192.pth --indir examples/demo/
#or if you want to use yolox-x as the detector
python scripts/demo_inference.py --detector yolox-x --cfg configs/coco/resnet/256x192_res50_lr1e-3_1x.yaml --checkpoint pretrained_models/fast_res50_256x192.pth --indir examples/demo/
```

Train `FastPose` on mscoco dataset.
``` bash
./scripts/train.sh ./configs/coco/resnet/256x192_res50_lr1e-3_1x.yaml exp_fastpose
```

More detailed inference options and examples, please refer to [GETTING_STARTED.md](docs/GETTING_STARTED.md)


## Common issue & FAQ
Check out [faq.md](docs/faq.md) for faq. If it can not solve your problems or if you find any bugs, don't hesitate to comment on GitHub or make a pull request!

## Contributors
AlphaPose is based on RMPE(ICCV'17), authored by [Hao-Shu Fang](https://fang-haoshu.github.io/), Shuqin Xie, [Yu-Wing Tai](https://scholar.google.com/citations?user=nFhLmFkAAAAJ&hl=en) and [Cewu Lu](http://www.mvig.org/), [Cewu Lu](http://mvig.sjtu.edu.cn/) is the corresponding author. Currently, it is maintained by [Jiefeng Li\*](http://jeff-leaf.site/), [Hao-shu Fang\*](https://fang-haoshu.github.io/),  [Haoyi Zhu](https://github.com/HaoyiZhu), [Yuliang Xiu](http://xiuyuliang.cn/about/) and [Chao Xu](http://www.isdas.cn/). 

The main contributors are listed in [doc/contributors.md](docs/contributors.md).

## TODO
- [x] Multi-GPU/CPU inference
- [ ] 3D pose
- [x] add tracking flag
- [ ] PyTorch C++ version
- [x] Add model trained on mixture dataset (Check the model zoo)
- [ ] dense support
- [x] small box easy filter
- [x] Crowdpose support
- [ ] Speed up PoseFlow
- [x] Add stronger/light detectors (yolox is now supported)
- [x] High level API (check the scripts/demo_api.py)

We would really appreciate if you can offer any help and be the [contributor](docs/contributors.md) of AlphaPose.


## Citation
Please cite these papers in your publications if it helps your research:

    @inproceedings{fang2017rmpe,
      title={{RMPE}: Regional Multi-person Pose Estimation},
      author={Fang, Hao-Shu and Xie, Shuqin and Tai, Yu-Wing and Lu, Cewu},
      booktitle={ICCV},
      year={2017}
    }

    @article{li2018crowdpose,
      title={CrowdPose: Efficient Crowded Scenes Pose Estimation and A New Benchmark},
      author={Li, Jiefeng and Wang, Can and Zhu, Hao and Mao, Yihuan and Fang, Hao-Shu and Lu, Cewu},
      journal={arXiv preprint arXiv:1812.00324},
      year={2018}
    }

    @inproceedings{xiu2018poseflow,
      author = {Xiu, Yuliang and Li, Jiefeng and Wang, Haoyu and Fang, Yinghong and Lu, Cewu},
      title = {{Pose Flow}: Efficient Online Pose Tracking},
      booktitle={BMVC},
      year = {2018}
    }



## License
AlphaPose is freely available for free non-commercial use, and may be redistributed under these conditions. For commercial queries, please drop an e-mail at mvig.alphapose[at]gmail[dot]com and cc lucewu[[at]sjtu[dot]edu[dot]cn. We will send the detail agreement to you.
