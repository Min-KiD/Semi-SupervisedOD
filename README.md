# Semi-Supervised Object Detection

This is our computer vision project 

report can be seen at `cv_semi.pdf`

Project aim: leverage both labeled and unlabeled data to improve the performance of object detection models, utilizing a combination of a small set of labeled data and a larger set of unlabeled data

## Dataset

- Full-Dataset: COCO(Common Objects in Context) 2017 version, for more information of dataset can be found here: https://cocodataset.org/#home

- 2017 version we use taken from kaggle which can be found here: https://www.kaggle.com/datasets/awsaf49/coco-2017-dataset

- Dataset we use is part of COCO 2017 version which only contain category vehicle mean 8 objects have id from 2 to 9
  you can find more information in this: https://www.kaggle.com/datasets/minmints/semis-od-coco-10/data, we divide by .json file
  filtered_instances_train2017.json is file for all annotations of our dataset
  instances_train2017_unlabeled_predicted.json in yolov9semi is file annotation predict by yolov9 have purpose for take advantage of unlabel dataset


## Infer

Alternative method, you can run our `semisod-infer.ipynb` for all of these, we have 2 ways of infer, one by SSD model, one by Retina model but based on detectron2 and it will run slower when building wheel for detectron2

weight for these 2 will be seen at https://www.kaggle.com/datasets/minmints/weightsod/data for SSD and https://www.kaggle.com/datasets/kid2108/weightretinatest/data for RetinaNet

## Models

We have experiment with FasterRCNN-R50, RetinaNet-R50, SSD300-VGG16, YoLov9,v10, DETR, Unbiased Teacher, Efficient Teacher

For teacher-student model, the teacher have been given to YoLov9-C, you can refer to `pseudo-labeling-semiod.ipynb` for generating label for unlabel dataset, we have both YoLov10-L and YoLov9-C. We recommend using Yolov9-C has better accuracy result

- FasterRCNN_R50 no pretrained is choosing with NMS and score threshold 0.5 and 0.75 with label data can be found at `Faster RCNN` folder, full is full data include unlabel data has been labeled by YoLov9-C fould at `YoLo` folder

- RetinaNet_R50 no pretrained is choosing with NMS and score threshold 0.15 and 0.3 with label data can be found at `Retina` folder, full is full data predicted by YoLov9-C

- DETR, Yolov9, v10 is advance model can be found at their folder

- SSD300_VGG16 is pretrained model version in folder, full is full data predicted by YoLov9-C

- Unbiased Teacher in code is 2000 steps but the result we achieve is by running 20700 steps, you increase the steps if you have enough hardware requirement GB

- Efficient Teacher is using YoLov5-L

## Some outstanding Results 

### Retina 1530 student and YoLov9 Teacher

<td><img alt="" src="./Retina/Retina1530/1530Full.png" />

`Retina 1530` model's mAP

### Unbiased Teacher with FasterRCNN-R50

<td><img alt="" src="./Unbiased Teacher/UT.png" />

`Unbiased Teacher` model's mAP
