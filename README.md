
# Table of Contents

1.  [Overview](#orgb642f8a)
2.  [Dataset](#org2d22c35)
    1.  [Preprocessing](#org050a6c2)
3.  [Model architecture](#orgd255c70)
    1.  [Encoder](#orgf0476c0)
    2.  [Decoder](#org5397eec)
4.  [Model evaluation](#org60f08ca)
5.  [Results](#org12dfe75)



<a id="orgb642f8a"></a>

# Overview

In the domain of computer vision, **image segmentation** is the process of partitioning a digital image into various *segments* or sets of pixels to make an image easier to analyze and process. This technique is particularly useful for determining object boundaries in a given image.
  
This technique assigns a **label** to each pixel and groups them together if they share same characteristic properties.  
In this project I use the `TensorFlow` backend for performing the task. I have also used the **M2NIST** dataset, which can is available on [Kaggle](https://www.kaggle.com/farhanhubble/multimnistm2nist).


<a id="org2d22c35"></a>

# Dataset

The **M2NIST** dataset is a popular choice for *multidigit semantic segmentation*. Since it is available in the form of `NumPy (.npy)` array files, preprocessing is done relatively easily. There are 2 image files in the dataset:

-   `combined.npy` which contains the images with multiple **MNIST** datasets
-   `segmented.npy` contains the segmentation masks corresponding to the combined file

This dataset has **5000** images, each of them having dimensions `(64x84)`. A random split of the data is performed using the corresponding `TensorFlow` API.


<a id="org050a6c2"></a>

## Preprocessing

The data is processed in **batches** with each one containing **32** images. Each of the images in our dataset is cast into the datatype `float32` whilst normalizing the pixel values across the RGB channels of the image.
  
Next, our data is split into the training and validation sets in an aribtrary split ratio that I chose to be `80:20` (for both the images and the corresponding annotations).


<a id="orgd255c70"></a>

# Model architecture

The heart of my implementation lies in the model architecture that I have used. I chose to use the **FCN-8** (Fully convolutional Network) architecture for this purpose. It remains a somewhat elementary and popular choice for problems concerning *semantic segmentation* of images.
  
The following diagram summarizes the basic picture of the entire model:

![img](./fcn8.png "The FCN-8 architecture")


<a id="orgf0476c0"></a>

## Encoder

I have used **2** `Conv2D` layers, each of which is followed by an activation using the `LeakyReLU` function. These layers are then **max pooled** followed by **normalization**. Diagramatically, the structure of the layers looks like so:
  
`Input -> Conv2D -> LeakyReLU -> Conv2d -> LeakyReLU -> MaxPooling2D -> Batch normalization`

This summarizes the building block of our encoder.


<a id="org5397eec"></a>

## Decoder

The decoder ties together **5 convolutional building blocks** which are used for *feature extraction* (apart from the fully connected layers).
  
An optimization (albeit *minor*) is introduced, where I have *resized* the image to have a dimension as a **power of 2**. For this purpose, I have used the builtin API of TensorFlow: `ZeroPadding2D`, which adds the necessary padding along the required dimensions to achieve my goal. Note that this must be done **only** along the width of the image, and not the height.


<a id="org60f08ca"></a>

# Model evaluation


<a id="org12dfe75"></a>

# Results

