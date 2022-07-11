# Retinal Vessel Segmentation Using GAN
The model produces a segmentation of the Retina image based using a Generative Adversarial Network trained on Perceptual Loss.

Human retina contains vast information on the presence of diseases in a humanbody. Any abnormalities in these tissues can help in the diagnosis of ophthalmicand cardiovascular diseases. This study process requires extracting a segmentedimage of the retinal vessel from the  retinal image, which makes the doctors workeasier. This paper presents an AI model to extract the segmentation from the retinal image.

1. Retinal Image
<p align="center">
  <img src="https://user-images.githubusercontent.com/58945374/117529679-7eda8d80-aff6-11eb-9514-653727ebb016.jpg" height="250" width="250"> 
</p>
2. Segmented Image
<p align="center">
  <img src="https://user-images.githubusercontent.com/58945374/117530592-9bc58f80-affb-11eb-855c-a2701044d94a.jpg" height="250" width="250">
</p>
 
# DATASET
The model is trained on 2 datasets, namely DRIVE and STARE.

1.DRIVE Dataset: The Digital Retinal Images for Vessel Extraction(DRIVE) dataset was established by the Niemeijer team based on the Dutch Diabetic RetinopathyScreening work in 2004. The dataset contains 40 atlases which wererandomly selected from 452 individuals. The resolution of each image is565*584. Out of those 40 atlases, 7 of them have early diabetic retinopathy.The dataset is divided into 2 subsets, 20 images in training and 20 images in thetest dataset.

2.STARE Dataset:The dataset was published in the paper by Hoover et al. in 2000. Thereare 20 images in the dataset out of which 10 have lesions. The resolution ofthe image is 605*700.
 
<a href="https://drive.google.com/drive/folders/1XwSii_o6JB2E1YqW4Gufv7JANefhPLxF?usp=sharing">Test Dataset</a>

<a href="https://drive.google.com/drive/folders/1hyyoPjl7hAi1_6iflWoc-ZIEixRkQ56e?usp=sharing">Train Dataset</a>

# Preprocessing Dataset
The images in the dataset are passed through a series of functions to enhance thequality of the image. By following these steps, the model can be trained quickly.The preprocessing techniques are

●Gray-Scaling

●Histogram Equalization

●Gamma Adjustment

# Generator Model:
The generator model is based on the U-Net architecture. The model contains 2 sections, an encoder and a decoder. 

The encoder structure is a simple CNN network. This part of the network extractsthe features of the image at each layer. The image is passed through a series of convolutions in each layer to extract different qualities of the image like edges,shapes, patterns, etc.

The decoder network can be thought of as a reverse CNN network. The network inthis part tries to project the features extracted from the image onto the pixel space,trying to create an image out of the features extracted. The network uses layers likeUp-sampling and convolution to adjust the size and features of the image to begenerated. The partial image generated by the previous decoder layer and thefeatures extracted in the encoder network are combined to generate the partialimage in the current layer.
<p align="center">
  <img src="https://user-images.githubusercontent.com/58945374/117538250-f0303580-b022-11eb-8a11-cb052eb955b4.png">
</p>

# Discriminator Model
The model uses 3 discriminators of the same structure. The main task of thediscriminator in a GAN model is to differentiate the manually segmented imagefrom the segment generated. The discriminator is expected to output “1” if amanually segmented image is passed to it and it is expected to output “0” otherwise.

## Global discriminator: 
The multiple discriminators take input of different sizes. The global usually takesthe image as it is, i.e the size is not reduced. This discriminator has a smallerreceptive field compared to the other 2 discriminators. The features extracted bythe layers in the global discriminator network is based only on the pixel values which the filter maps. The discriminator is successful in finding differences at eachlocation between the manually segmented and segment generated by the model.
## Multi Discriminator 1:
The input image to this network is generally down-sampled by a factor of 2. Thisdown-sampling helps the network take features from a large receptive field. Theactivations calculated are based on both the current pixel values and theneighbouring features. With the same filter size, a down-sampled image providesmore information on neighbouring pixels than the original image. The networkstructure is the same as the global discriminator.

## Multi Discriminator 2:
The input image is further down-sampled by a factor of 2, i.e down-sampled by afactor of 4 compared to the original image. This network has an even betterreceptive field compared to the other 2 discriminators.

# LOSS FUNCTION

## Perceptual Loss:
Perceptual loss or content loss is calculated as the squared-error between eachactivation generated at each layer when the generated and the originally segmentedimage are passed to a pre-trained CNN model. Any simple network trained forimage classification can be used for calculating the loss.

The pre-trained network extracts various features out of both the images passed toit, at different layers. So if the generated image is close to the manually segmentedimage, the difference in the features of the image is low, The formula forperceptual loss is shown below:
<p align="center">
  <img src="https://user-images.githubusercontent.com/58945374/117538396-9bd98580-b023-11eb-82e9-6bda15982266.png">
</p>

A LeNet model is trained for hand-signal number recognition problems. This is themodel used to calculate the perceptual loss of the model.

## Discriminator Loss:
The generator loss uses the output of the discriminators to judge the generation ofthe image. So it becomes necessary that the discriminators are trained ahead of thegenerator. The training process of the discriminators occurs on the “binary crossentropy” loss. The discriminator outputs are expected to be either 0 or 1, hence ona batch of both manually segmented and system generated images, the loss is ametric on how well the discriminator has differentiated.

# Results:
For a retinal Image in the STARE dataset, the following segmentation in achievedby the following models:

<p align="center">
  <img src="https://user-images.githubusercontent.com/58945374/117538613-9cbee700-b024-11eb-8166-10fd3a9f0bc7.png">
</p>
