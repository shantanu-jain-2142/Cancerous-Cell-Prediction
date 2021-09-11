## Cancerous-Cell-Prediction
Developed two machine learning models for identification
of cancerous regions within a cell based on gigapixel pathology images. 
The dataset was obtained from Camelyon-16.

### Task
- The task is to achieve automated detection of metastases in hematoxylin and eosin (H&E) stained whole-slide images of lymph node sections.
- Given a collection of images, develop a machine
learning model that outputs a heatmap showing 
regions of a biopsy likely to contain cancer.

### Dataset
- The dataset is obtained from the [CAMELYON 16](https://camelyon16.grand-challenge.org/Data/)
challenge.
- The original dataset contains 400 whole slide images,
but for the purposes of our model, we use only 21 such WSI's.
- Each WSI is available in 8 zoom levels, going from 1x to 40x magnification.
- The magnification is performed from the centre, and the resolution for higher
zoom levels is too large to load into memory.
- We show that with the help of "patching", we can
augment the dataset to attain a significant amount
of training examples.

### Preprocessing
![patching-process](/Images/Patching.png)

#### Patching
- Data Augmentation using Patching. Patching is the process of segmenting
the gigapixel image into small chunks of images of a fixed size. 
- This allows us to obtain a significant amount of small images from a single
gigapixel image.
- Doing this for multiple zoom levels, allows us to augment the dataset even
further.

#### Centre Alignment
- The issue with patching from multiple zoom levels is that the patch corresponding
to the same region may not be aligned. 
- Hence, alignment is required to ensure that patches corresponding to the same region
potray similar information for the model to take advantage of.

#### Gray region removal
- I observed that major portion of the biopsy images had gray background around
the tissue cells, which do not contribute to the actual training.
- Hence it was important to remove patches which majority gray background so as
to improve accuracy, as well as reducing training overhead.

### Custom Model with one zoom level
![Custom Model](/Images/custom_model_1_zoom.png)

- Zoom Level:  4
- Patch size: 32
- Epochs: 10
- Thresholding: 0.5
- Training Patches: 146570

#### Prediction
![Custom Model Prediction](/Images/custom-model-prediction.png)

### 2-tower Inception V3 with Two zoom levels
![Inception Model](/Images/inception-model.png)

- Zoom Level:  3 and 4
- Patch size:75
- Epochs: 10
- Thresholding: 0.5
- Training Patches: 30065

#### Prediction
![Inception Model Prediction](/Images/inception-prediction.png)

### Demo
[Demo Presentation](https://youtu.be/Q9BGDConXhI)

### Conclusion
- In terms of accuracy, f1-score and IOU measure, the custom model and InceptionV3 gave similar performance'
 on the 2 slide test set that we had.
- But InceptionV3 performed better, since it was able to achieve the same level of predictability, with only 30065 patches,
 as compared to 146570 patches for the custom model.
- Hence, even though the performance is similar, InceptionV3 outperforms the custom model in terms of data complexity.
