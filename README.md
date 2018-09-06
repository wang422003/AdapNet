# AdapNet:  Adaptive  Semantic  Segmentation in  Adverse  Environmental  Conditions
AdapNet is a deep learning model for semantic image segmentation that is benchmarked on Cityscapes, Synthia, SUN RGB-D, ScanNet and Freiburg Forest datasets.

This repository contains our TensorFlow implementation of AdapNet which allows you to train your own model on any dataset and evaluate the results results in terms of mIoU (mean intersection-over-union). 

If you find the code useful for your research, please consider citing our paper:
```
@inproceedings{valada2017icra,
author = "Valada, Abhinav and Vertens, Johan and Dhall, Ankit and Burgard, Wolfram",
title = "AdapNet: Adaptive Semantic Segmentation in Adverse Environmental Conditions",
booktitle = "Proceedings of the IEEE International Conference on Robotics and Automation (ICRA)",
pages={4644--4651},
month = "May",
year = "2017",
}
```

## Example Segmentation Results:

| Dataset       | RGB_Image     | Segmented_Image|
| ------------- | ------------- | -------------  |
| Cityscapes    |<img src="images/city.png" width=400> |  <img src="images/city_v1.png" width=395>|
| Forest  | <img src="images/forest.png" width=400>  |<img src="images/forest_v1.png" width=395> |
| Sun RGB-D  | <img src="images/sun.png" width=400>  | <img src="images/sun_p.png" width=395>|
| Synthia  | <img src="images/synthia.png" width=400>  | <img src="images/synthia_v1.png" width=395> |
| Scannet v2  | <img src="images/scannet.png" width=400>  |<img src="images/scannet_pr.png" width=395> |
## System requirement

#### Programming language
```
Python 2.7
```
#### Python Packages
```
tensorflow-gpu 1.4.0
```
## Configure the network

Download the resnet_v1_50 tensorflow pre-trained model for network intialization from [here](https://github.com/tensorflow/models/tree/master/research/slim).
#### Data

* Augment the default dataset -> augmented-training-dataset.
  In our work, we first resized the images in the dataset to 768x384 pixels and then apply a series of augmentations.
  (random_flip, random_scale and random_crop)

* Convert augmented-training-dataset/val-dataset/test-dataset into the .tfrecords format.
  Create a .txt file having entries in the following format:
  ```
     path_to_modality1/0.png path_to_label/0.png
     path_to_modality1/1.png path_to_label/1.png
     path_to_modality1/2.png path_to_label/2.png
     ...
  ```
  Run the convert_to_tfrecords.py from dataset folder to create the tfrecords and mean '.npy' file:
  ```
     python convert_to_tfrecords.py --file path_to_.txt_file --record tf_records_name.tfrecords --mean 1
  ```
  (Input to model is in BGR and 'NHWC' form)

#### Training
```
    gpu_id: id of gpu to be used
    model: name of the model
    num_classes: number of classes
    intialize:  path to pre-trained model
    checkpoint: path to save model
    train_data: path to dataset .tfrecords
    batch_size: training batch size
    skip_step: how many steps to print loss 
    height: height of input image
    width: width of input image
    max_iteration: how many iterations to train
    learning_rate: initial learning rate
    save_step: how many steps to save the model
    power: parameter for poly learning rate
    mean: path to mean file 
```
#### Evaluation
```
    gpu_id: id of gpu to be used
    model: name of the model
    num_classes: number of classes
    checkpoint: path to saved model
    test_data: path to dataset .tfrecords
    batch_size: evaluation batch size
    skip_step: how many steps to print mIoU
    height: height of input image
    width: width of input image
    mean: path to mean file 
```

## Training and Evaluation

#### Start training
Create the config file for training in config folder.
Run
```
python train.py -c config cityscapes_train.config or python train.py --config cityscapes_train.config

```

#### Eval

Select a checkpoint to test/validate your model in terms of mean IoU.
Create the config file for evaluation in config folder.

```
python evaluate.py -c config cityscapes_test.config or python evaluate.py --config cityscapes_test.config
```
## Additional Notes:
   * We provide only the single scale evaluation script. Multi-Scale+Flip evaluation further imporves the performance of the model.
   * The code in this repository only performs training on a single GPU. Multi-GPU training using synchronized batch normalization with larger batch size furthur improves the performance of the model.

