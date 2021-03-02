# BrainTumorSegmentation-CNN
Implementation of deep CNN model that was trained to segment the tumor on 3D MRI images. The model was inspired from the ResNet architecture and classified each pixel to be in one of the 5 classes- necrosis, edema, enhancing, non-enhancing tumor, normal tissues. The model was trained and evaluated using the BRATS 2013 dataset and obtained the DICE coefficient of 0.92.

## Dataset
The Training dataset used was obtained from the MICCAI segmentation challenge conducted in 2013. The dataset is named after the challenge BRAin Tumor Segmentation (BRATS) 2013.
MR imaging allows using different modalities for mapping tumor-induced tissue changes, such as T2 and FLAIR MRI (highlighting differences in tissue water relaxational properties), post-Gadolinium T1 MRI (showing pathological intratumoral take-up of contrast agents), perfusion and diffusion MRI (local water diffusion and blood flow) and some more.
BRATS 2013 dataset consist of images with 4 modalities Fluid Attenuated Inversion Recovery (FLAIR), T1, T1-contrasted, and T2.
It consists MRI images of 20 patients with either of anaplastic astrocytomas or glioblastoma multiforme tumors. There are a total of 155 slices in each of the 4 modalities of scanning for a single patient with manually segmented tumors in all the slices . Hence each patient contributes about 620 images in the dataset.

## Pre-processing of MR images
Pre-processing is important because of the artifacts produced either by inhomogeneity in the magnetic field or small movements made by the patient during scan time.
This results in low frequency intensity non-uniformity to be present in MRI image data and is known as a bias or gain field.
To remove this bias from the training images, a standard bias correction algorithm called N4 was used1.
However, applying only bias correction was not enough to rectify the contrast in same tissues across various slices of the MRI scans. Hence an intensity normalization (mean, std-dev) step was also performed after bias correction.
MRI slices mostly represented healthy tissues with tumorous tissues making only a small part of the slice. This creates imbalance in the classes of segmentation during training a model.

## Patch extraction of MR images
An approach to make the training data balanced is to train the model on patches extracted from the training data.
The ground truth consist of 5 classes- necrosis (dead tissues), edema (swelling), enhancing tumor, non-enhancing tumor, normal tissues. 
The model needs to emphasize more on learning the features surrounding a tumorous region and hence a 33x33 sized patch was extracted across a central pixel value corresponding to one of the 5 classes mentioned above.
The number of patches extracted where same across all classes thereby making the model input class-balanced.
The patches were extracted across all 4 modalities and hence the dimension of model input became 4x33x33.
The patches were also randomly augmented (rotation augmentation) as a regularization step.

![image](https://user-images.githubusercontent.com/36618302/109579678-5f3f5a00-7ac7-11eb-83e2-bdecf7497163.png)

## Model Architecture
![image](https://user-images.githubusercontent.com/36618302/109579771-88f88100-7ac7-11eb-9599-a799a3c9dd41.png)

## Evaluation Criteria and Color Coding
### Dice Similarity Coefficient
Used to measure the spatial overlap between the segmented output and ground truth image.
Dice similarity coefficient for the complete segmented output of the model is not calculated rather only the regions related to the tumor core (necrosis, enhancing tumor and non-enhancing tumor), enhancing tumor and complete tumor (except background and healthy tissues) are calculated.
![image](https://user-images.githubusercontent.com/36618302/109579923-c65d0e80-7ac7-11eb-8b3d-20cfccef4897.png)

### Color Coding the Tumor classes 
Different Tumor classes were coded with different color for visual comparison between prediction and ground truth.
![image](https://user-images.githubusercontent.com/36618302/109579938-cbba5900-7ac7-11eb-91c1-4426ae0d5df3.png)

## Results

