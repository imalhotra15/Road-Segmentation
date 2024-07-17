# Road Segmentation Using VGG-FCN8 on KITTI Dataset

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Dataset](#dataset)
4. [Model Architecture](#model-architecture)
5. [Training the Model](#training-the-model)
6. [Evaluating the Model](#evaluating-the-model)
7. [Testing on KITTI Dataset](#testing-on-kitti-dataset)
8. [Visualization](#visualization)
9. [Code Organization](#code-organization)
10. [Usage](#usage)
11. [Saving and Loading Models](#saving-and-loading-models)
12. [Results](#results)
13. [Troubleshooting](#troubleshooting)
14. [Contributing](#contributing)
15. [License](#license)
16. [Acknowledgements](#acknowledgements)
17. [Contact Information](#contact-information)

## Introduction
This project focuses on segmenting roads from aerial images using a VGG-FCN8 neural network. The primary goal is to accurately identify road pixels to aid in autonomous driving and GIS applications.

## Prerequisites
### Software Requirements
- Python 3.8+
- PyTorch 1.8+
- torchvision
- OpenCV
- numpy
- PIL

### Hardware Requirements
- GPU with CUDA support (for training)

### Installation Guide
```bash
# Clone the repository
git clone https://github.com/imalhotra15/road-segmentation.git

# Navigate to the project directory
cd road-segmentation
```
### Dataset
The dataset consists of aerial images with corresponding road masks.

#### Structure
```bash
data_road/
│
└─── training/
│    └─── gt_image_2/  <--------- Masks
│      └─── image1.png
       └─── image2.png
     └─── image_2/    <---------- Raw
│      └─── image1.png
       └─── image2.png

```
### Model Architecture
#### VGG-FCN8 Overview

![image](https://github.com/user-attachments/assets/bcde7b3b-b2b3-418f-bd28-edbe77c9fc70)

The VGG-FCN8 model is a fully convolutional network based on the VGG16 architecture. It adapts the VGG16 network for segmentation tasks by adding upsampling layers to produce pixel-wise predictions.

#### Encoder and Decoder Explanation
##### Encoder
The encoder part of the VGG-FCN8 model uses the VGG16 architecture pre-trained on ImageNet. It consists of 5 blocks of convolutional layers followed by max-pooling layers.

- **Block 1-3**: These layers extract lower-level features from the input image.
- **Block 4**: This layer captures mid-level features.
- **Block 5**: This layer captures high-level features.

The encoder is divided into three segments to facilitate skip connections:
- `enc1`: Block 1 to Block 3
- `enc2`: Block 4
- `enc3`: Block 5

##### Decoder
The decoder part of the VGG-FCN8 model upsamples the feature maps to the original image size using transposed convolutions. The decoder also incorporates skip connections from the encoder to retain spatial information.

- **Upsampling Layer 1 (up1)**: Upsamples the feature maps from `enc3` by a factor of 2.
- **Concatenation 1 (d1)**: Concatenates the upsampled feature maps from `up1` with the features from `enc2`.
- **Upsampling Layer 2 (up2)**: Upsamples the concatenated feature maps by a factor of 2.
- **Concatenation 2 (d2)**: Concatenates the upsampled feature maps from `up2` with the features from `enc1`.
- **Upsampling Layer 3 (up3)**: Upsamples the concatenated feature maps by a factor of 8 to reach the original image size.

#### Model Modifications
The VGG-FCN8 model was modified to accommodate binary road segmentation. The modifications include:
- Using only one output channel (binary classification) instead of 21 (original FCN8 used for PASCAL VOC).
- Adding transposed convolution layers to upsample the feature maps back to the original image size.
- Incorporating skip connections to combine lower-level and higher-level features.

### Results

#### Training Loss Plots

![image](https://github.com/user-attachments/assets/22f29ddd-4804-4309-b238-fafd751d242e)

#### Training Visualization

https://github.com/user-attachments/assets/dc096a27-77e7-48d5-9a65-13348d18693a


#### Video Demo

https://github.com/user-attachments/assets/86cb15cc-eb12-4177-b274-4f6f14a01c61











