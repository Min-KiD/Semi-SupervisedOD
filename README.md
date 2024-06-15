# Semi-Supervised Object Detection

This is our computer vision project 

link drive for report and weight of models: https://drive.google.com/drive/folders/1WCGLPBoWY2ovkOcNZLrQ5jC0fDF_u5uJ

Project aim: leverage both labeled and unlabeled data to improve the performance of object detection models, utilizing a combination of a small set of labeled data and a larger set of unlabeled data

## Dataset

- Full-Dataset: COCO(Common Objects in Context) 2017 version, for more information of dataset can be found here: https://cocodataset.org/#home

- 2017 version we use taken from kaggle which can be found here: https://www.kaggle.com/datasets/awsaf49/coco-2017-dataset

- Dataset we use is part of COCO 2017 version which only contain category vehicle mean 8 objects have id from 2 to 9
  you can find more information in this: https://www.kaggle.com/datasets/minmints/semis-od-coco-10/data, we divide by .json file
  filtered_instances_train2017.json is file for all annotations of our dataset
  instances_train2017_unlabeled_predicted.json in yolov9semi is file annotation predict by yolov9 have purpose for take advantage of unlabel dataset


## Infer

Step 1: Go to the small dataset https://www.kaggle.com/datasets/mission-ai/crimeucfdataset then create notebook or full-dataset one https://www.kaggle.com/datasets/minmints/ufc-crime-full-dataset, set the accelerator to GPU T4 x2

Step 2: Download weight of model

```python
import requests
import os

url = 'https://drive.google.com/uc?id=1-2EQRKO0jUnGPhU_3GuqtyAyu4FS93BR&export=download&confirm=t&uuid=12eaf101-0796-4f5b-813b-cbe20b5dbde0'
save_dir = '/kaggle/working/'
response = requests.get(url)

with open(os.path.join(save_dir, 'model.pth'), 'wb') as f:
    f.write(response.content)
    
url = 'https://drive.google.com/uc?id=1L6Z5df37lb7LDiag17MB3XCrQfwBcGeU&export=download&confirm=t&uuid=12eaf101-0796-4f5b-813b-cbe20b5dbde0'
response = requests.get(url)

with open(os.path.join(save_dir, 'RGB_Kinetics_16f.pth'), 'wb') as f:
    f.write(response.content)

```
Step 3: Infer, '/kaggle/input/crimeucfdataset/Anomaly_Dataset/Anomaly_Videos/Anomaly-Videos-Part-1/Arrest/Arrest002_x264.mp4' is the file_path, you can change to file_path you want to predict

```python
!git clone https://github.com/Min-KiD/DLProject-AnomalyDetected
%cd DLProject-AnomalyDetected/infer
!python /kaggle/working/DLProject-AnomalyDetected/infer/infer.py '/kaggle/input/crimeucfdataset/Anomaly_Dataset/Anomaly_Videos/Anomaly-Videos-Part-1/Arrest/Arrest002_x264.mp4'
```

Alternative method, you can run our `demovida.ipynb` for all of these

## Feature Extraction for Anomaly Detection

we use I3D for Spatial-Temporal 32 segments feature extraction with output is rgb and optical flow numpy file

- Input
The inputs are paths to video files. Paths can be passed as a list of paths or as a text file formatted with a single path per line.

- Output
Output is defined by the on_extraction argument; by default it prints the features to the command line. Possible values of output are ['print', 'save_numpy', 'save_pickle']. save options save the features in the output_path folder with the same name as the input video file but with the .npy or .pkl extension.

Read more and run in file `feature-extraction.ipynb`

## Training for Anomaly Detection 

After finishing extract feature for full-dataset, we will use it for training, it already has been uploaded to Kaggle: https://www.kaggle.com/datasets/kanishkarav/ucf-crime-video-dataset

File `model.ipynb` will train then save weight model

## Some Results 

### Classification

<td><img alt="" src="./Classification/roc_curve_s.png" />

`x3d_s` model's roc curve

<td><img alt="" src="./Classification/roc_curve_xs.png" />

`x3d_xs` model's roc curve


### Anomaly Detection 

<td><img alt="" src="./Detection/media_images_ROC Curve.png" />

## Demo

File `DemoVidA.ipynb`, remember to check the path to weight of the model and its output path if you want to run this

<table>
  <tr>
    <td><img alt="" src="./Arrest002gif.gif" /></td> <td><img alt="" src="./Arrest002_x264_result.png" height="280" width="400" />
  <tr>
</table>
