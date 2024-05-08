---
title: "Semantic Segmentation of Aerial Images"
excerpt: "A comparison of UNET versus SegNET for Land Cover Segmentation<br/><img src='/images/segmentation.png'>"
collection: portfolio
---

Overview
------
In this project, an implementation of a Fully Convolutional Network for semantic segmentation of aerial imageries was carried out. The idea was to automatically perform scene interpretation of images taken from a plane, UAV, or a satellite by classifying every pixel into several land cover classes. The baseline implementation of the [SegNet model](https://hal.science/hal-01636145) presented in the scientific work of Nicolas Audebert, Bertrand Le Saux and Sébastien Lefèvre were utilized and compared to the [UNET model](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/).


Semantic segmentation is considered one of the main tasks of computer vision. Any task or algorithm that associates a label or a category to every pixel in an image is considered **semantic segmentation.** This task has many numerous applications earth observation, medical imaging as well as in autonomous driving. 


Datasets
------
The Vaihingen <https://www2.isprs.org/commissions/comm2/wg4/benchmark/2d-sem-label-vaihingen/> is a well-known dataset used of for urban semantic segmentation staring from the ISPRS 2D Semantic Labeling Contest - Vaihingen. The datasets is available at the ISPRS Challenge website.

**Dataset format:**
* images are 3-channel RGB geotiffs
* masks are 3-channel geotiffs with unique RGB values representing the class

**Dataset classes:**

0. Clutter/background
1. Impervious surfaces
2. Building
3. Low Vegetation
4. Tree
5. Car

Model architectural setups
-----
### SegNet
Segnet is a deep convolutional encoder-decoder architecture for image segmentation. It takes an image as an input and returns a pixel-wise label map as output. Through series of convolutional and pooling layer, the encoder captures high-level features in the image, while the decoder uses the spatial pooling indices, obtained from the max-pooling in the encoder phase, to upsample and produce a segmentation map. This archicture provides a memory-efficient way of preserving the fine-grained details in the output map. 

![SegNet Architecture](/images/segnet_architecture.png)
Figure: SegNet Architecture *adapted from: [SegNet: A Deep Convolutional Encoder-Decoder Architecture for Image Segmentation](https://arxiv.org/pdf/1511.00561.pdf)*


The encoder from SegNet is based on the convolutional layers from VGG16 [38]. It has 5 convolution blocks, each containing 2 or 3 convolutional layers of kernel 3 × 3 with a padding of 1 followed by a rectified linear unit (ReLU) and a batch normalization (BN) [39]. Each convolution block is followed by a max-pooling layer of size 2 × 2. Therefore, at the end of the encoder, the feature maps are each W32×H32 where the original image has a resolution W × H.

The decoder performs both the upsampling and the classification. Its structure is symmetrical with
respect to the encoder. Pooling layers are replaced by unpooling layers.  The unpooling relocates the activation from the smaller feature maps into a zero-padded upsampled map. The activations are relocated
at the indices computed at the pooling stages, i.e. the argmax from the maxpooling.

Weight Initialization: Kaiming Normal 
Batch Normalization: 
Activation Function: ReLU
Encoder: VGGNet




### UNET
The UNET Architecture was initial proposed for medical image segmentation problems but soon adapted for other tasks. It is uniquely suited for tasks with high resolution images as inputs and outputs. And has gained used in image segmentation, super-resolution, and diffusion models. The UNET model is an Encoder-Decoder architecture, where the encoder extracts features from the input image and the decoder upsamples intermediate features and produces the final output. The Encoder and the Decoder are symmetrical and are connected by pads, hence the name alias the U. 

a convoluttional neural network with an encoder-decoder type architecture. 

The Encoder: consists of repeated convolution layers and max-pooling layers that extracts intermediate features. 



uses the skip connection to 

It consists of a contracting path to capture context and an expansive path for precise localization. The skip connections between the encoder and decoder help maintain spatial information.


Results
-----

|                    | UNET (F1-Score)  | SegNET (F1-Score) |
|--------------------|------------------|-------------------|
| Roads              |     0.87129      |     0.78039       |
| Buildings          |     0.91787      |     0.85471       |
| Low Vegetation     |     0.64783      |     0.59591       |
| Trees              |     0.84443      |     0.78069       |
| Cars               |     0.60164      |     0.03679       |
| Clutter            |       NaN        |       NaN         |
| **Total Accuracy** |     87.95856%    |     80.21025%     |
| **Kappa Score**    |     0.80214      |     0.67693       |


Reference
----

[Code Repository: Semantic Segmentation of Aerial Imageries](https://github.com/Ruphai/UBS/blob/main/Computer%20Vision/Image%20Analysis/SemSegmentation.ipynb)

[What is semantic segmentation?](https://www.mathworks.com/solutions/image-video-processing/semantic-segmentation.html#:~:text=Semantic%20segmentation%20is%20a%20deep,pixels%20that%20form%20distinct%20categories.)

