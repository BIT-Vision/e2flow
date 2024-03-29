# e2flow

This repository contains the official codes for our paper:

```Preserving Motion Detail in the Dark: Event-enhanced Optical Flow Estimation via Recurrent Feature Fusion```

## Requirements

You can install the Python and Pytorch environment by running the following commands:

```
conda create --name e2flow
conda activate e2flow
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.7 -c pytorch -c nvidia
```

Then, you can install the necessary Python libraries using the following command:

```
pip install -r requirements.txt
```

## Checkpoints

You can get checkpoint of our model from [e2flow-chairsDark](https://drive.google.com/drive/folders/14lrhoKdycVyfgtUlWHTkn6RV6JwNpNS6).

Meanwhile, we have gathered some published model weights for comparative experiments. You can get them from  [Checkpoints](https://drive.google.com/drive/folders/1rW_M4aqLmHve7GN19sC0BZeTpzF96olM).

We suggest organizing the checkpoints as follows:

```
|- checkpoints
|    |- e2flow
|    |    |- e2flow-chairsDark.pth
|    |- raft
|    |    |- chairs.pth
|    |    |- things.pth
|    |- ...
```

## Data Preparation

You need to download datasets to the corresponding folder and organize them correctly.

```
|- datasets
|    |- FlyingChairs2
|    |    |- ... 
|    |- MVSEC
|    |    |- ... 
|    |- RealData
|    |    |- ... 
```

Alternatively, you can customize the dataset path in [configs.py](configs.py).

### FlyingChairs-Dark

You can download the [FlyingChairs](https://lmb.informatik.uni-freiburg.de/resources/datasets/FlyingChairs.en.html#flyingchairs) dataset, and then generate simulated data based on the simulation baseline proposed in the paper. 

You can also directly download simulated data from [FlyingChairsDark](https://pan.baidu.com/s/1qKwYDzQUGOKET49JpTDJpQ?pwd=s1di).

Completely, the dataset should be organized as following format:

```
|- train
|    |- dark
|    |    |- 0000000-img_0.npy
|    |    |- 0000000-img_1.npy
|    |    |- ...
|    |    |- 0022231-img_0.npy
|    |    |- 0022231-img_0.npy
|    |- 0000000-flow_01.flo
|    |- 0000000-img_0.png
|    |- 0000000-img_1.png
|    |- ...
|    |- 0022231-img_1.png
|- val
|    |- dark
|    |    |- 0000000-img_0.npy
|    |    |- ...
|    |    |- 0000639-img_1.npy
|    |- 0000000-flow_01.flo
|    |- ...
|    |- 0000639-img_1.png
|- voxels_train_b5_pn.hdf5
|- voxels_val_b5_pn.hdf5
```

### MVSEC
You can download [MVSEC](https://daniilidis-group.github.io/mvsec/) in hdf5 format from [Google Drive](https://drive.google.com/drive/folders/1rwyRk26wtWeRgrAx_fgPc-ubUzTFThkV).

The dataset should be organized as following format:

```
|- data_hdf5
|    |- indoor_flying1_data.hdf5
|    |- indoor_flying1_gt.hdf5
|    |- indoor_flying1_gt_index.hdf5
|    |- ...
|    |- outdoor_day2_data.hdf5
|    |- outdoor_day2_gt.hdf5
|    |- outdoor_day2_gt_index.hdf5
```

###  Real-world low light dataset

You can get it from [RealData](https://pan.baidu.com/s/1HTlGbnVEaLctz-lZsnU6WQ?pwd=5g2t).

Similarly, the files should be organized as following format:

```
|- Indoor_1_a.h5
|- Indoor_1_b.h5
|- Indoor_2_a.h5
|- ...
|- Outdoor_3_d.h5
```

## Training

For simplicity, you can run the following command to train e2flow with FlyingChairsDark dataset.

```
python /data/zhangpengjie/zhangpengjie/Workspace/Experiments/e2flow/train.py
```

If you want to adjust the training parameters, please modify [configs.py](configs.py).

## Testing

You can evaluate e2flow on FlyingChairsDark-val with the following command, and the outcomes will be saved in `/outcomes/e2flow/f2` by default.

```
python /data/zhangpengjie/zhangpengjie/Workspace/Experiments/e2flow/test.py
```

If you want to evalute e2flow on other datasets, you can run the following commands:

```
python test.py --datasetName mvsec --sequence indoor_flying1
python test.py --datasetName real --sequence Indoor_3_a
```

We also provide instructions to evaluate other models:

```
python test.py --modelName raft --checkpoint ./checkpoints/raft/raft-chairs.pth
python test.py --modelName flowformer --checkpoint ./checkpoints/flowformer/chairs.pth
python test.py --modelName dcei --checkpoint ./checkpoints/dcei/DCEIFlow_paper.pth
```

## Acknowledgments

Thanks for the following helpful open source projects: [DCEI](https://github.com/danqu130/DCEIFlow), [ERAFT](https://github.com/uzh-rpg/E-RAFT), [TMA](https://github.com/ispc-lab/TMA), [RAFT](https://github.com/princeton-vl/RAFT), [FlowFormer](https://github.com/drinkingcoder/FlowFormer-Official), [v2e](https://github.com/SensorsINI/v2e).
