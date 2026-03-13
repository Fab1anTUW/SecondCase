# SecondCase

This project is an adaption of the original SecondPose, to work with a synthetic luggage dataset

## Requirements
The original requirements were changed due to incompabilities/problems.
The code has been tested with
- python 3.10
- pytorch 2.3
- CUDA 12.1
Other dependencies:

```
requirements.txt
```

Setup env:

```
conda create -n secondpose python=3.10
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
cd lib/pointnet2/
pip install .
cd ../sphericalmap_utils/
pip install .
cd ../../
pip install -r requirements.txt
pip install open3d
```

If error "ImportError: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.32' not found" with pointnet2 or spherical occurs, try:

- conda install gxx_linux-64
- cd lib/pointnet2/
- rm -rf build
- rm -rf pointnet2/_ext.cpython-*-x86_64-linux-gnu.so

## Data Processing

1. Please refer to the Self-DPDN adaptation [Self-DPDN-SecondPose](https://github.com/Fab1anTUW/Self-DPDN-SecondCase)
2. The Self-DPDN was adapted to work with the luggage dataset. For detail view the Self-DPDN-luggage README
3. run data_preprocess.py
4. Check the camera intrinsics in dataset_category & solver_category

## Data Layout

The data layout is the same as in the NOCS datasets and should look something like this:

NOCS
-camera
  -train
  -val
  ...
- detection (for eval)
- obj_models

Self-DPDN-luggage should correctly format the dataset.

## Network Training
Train SecondPose for rotation estimation:

```
python train_geodino.py --gpus 0 --mod r
```

Train the network of [pointnet++](https://github.com/charlesq34/pointnet2) for translation and size estimation:

```
python train.py --gpus 0  --mod ts
```


## Evaluation

For the evaluation prepare a inference dataset using Self-DPDN. (Details in the readme)
If the inference images were added afterwards use data_preprocess.py and check if camera_test_stats = load_stats_test... is not active/not commented.


To test the model, please run:

```
python test_geodino.py --gpus 0 --test_epoch [YOUR EPOCH]

```
In solver_category.py with the function test_func you can select if the results should be drawn. The drawn results are saved under log/CAMERA25/results/VI_Net_Geodino/epoch_[YOUR EPOCH]. New drawn results are only produced if the folder of the corresponding epoch does not exist.


## Acknowledgements

This repo is an adaption of the original SecondPose code, for the RobotVision lecture. [SecondPose](https://github.com/NOrangeeroli/SecondPose)

