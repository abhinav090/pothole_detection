Pothole Detection via MaskRCNN
==============================

This whole Project was primarly build for the **__TATA innoverse competition 2018
(November) Pothole Detection Competition__**


## Abstract
This Project is based upon collection of images from phone application's and usinng Deep Learning
Technique( Mask RCNN ), we have trained a custom machine learning model for detection of potholes
inside images and plot them on google maps,which includes the size of the pothole inside image.

Pothole detection Accuracy **__81%__** in different environments

The main files are ````Final_file_for_tata_innoverse.ipnb````

if you require to train your own custom Machine learning model from the same dataset
run this simple command referenced from the 
```
python3 custom.py train --dataset=customImages --model=/path/to/weights.h5
```

![Pothole Detected](https://media.giphy.com/media/Xc9DFuCIIiouihgfzZ/giphy.gif)  ![Original Images Link](https://media.giphy.com/media/jpKRIVzNVJv3VEegTZ/giphy.gif)


## Team

- [Pradyum Gupta](https://www.linkedin.com/in/pradyumgupta/)
- [Surya Pratap Shekhawat](https://www.linkedin.com/in/surya-pratap-shekhawat-906147159/)
- [Avinash Kumar](https://www.linkedin.com/in/avinashme/)


## SETUP
We have used variety of Datasets  :

Install **youtube-dl** to __download files from google drive__ directly in terminal
```
https://github.com/ytdl-org/youtube-dl#installation
```
#### Training Dataset
To increase the model accuracy , For the training dataset we have trained them _5 types of dataset.._
- Original Dataset Links
```
Sunny Dataset
Anue Dataset
Road Damage Dataset (Adachi)
Rick_set6
Kitti image Dataset
```
If you want to add your own dataset into our dataset. We have **resized all the images** for faster processing to **800x800**, Please do same with your images too
```python
from PIL import Image
import os
from resizeimage import resizeimage

count = 0

for f in os.listdir(os.getcwd()):
    try:
        with Image.open(f) as image:
            count +=1
            cover = resizeimage.resize_cover(image, [800,800])
            cover.save('pgm-1_{}.jpg'.format(count),image.format)
            print(count)
            os.remove(f)
    except(OSError) as e:
        print('Bad Image {}{}'.format(f,count))
```


We have also done annotion of all the images present there,
Annotion images

- For Annotion of images we have used robots.ox.ac.uk
````
http://www.robots.ox.ac.uk/~vgg/software/via/via-1.0.6.html
````
**__After annotion around 10,000 images then we create this dataset__**


**Download these files**

| Folder Name        | Download Link           |
| -------------------|:-----------------------:|
| Train | [Gdrive link](https://drive.google.com/file/d/1--0RB8oXNY876IushT8Z60Trl8_RNO11/view?usp=sharing) |
| Val      | [Gdrive link](https://drive.google.com/file/d/1PGhXUnaJDpcjgoLEhNn9gRvGZGRCzWUt/view?usp=sharing)     |

We have created this Dataset for final testing of the model
This folder contains videos and photos that we recorded in May 2019
**[Test Dataset Download Link](https://drive.google.com/drive/u/2/folders/1duZ9O0If8mpHk8lZkFHQifv5R8z4dcKx)**
```

**For custom Training Process**
````
extract the zip files and create a folder named as "customImages" and move both folders
(train) & (val) folder directly inside it and then run the command

Structure for the whole Test Dataset Links
````
├── test_dataset
├──----- Photos
├──----- IMG_7300.MOV
├──----- IMG_7301.MOV
├──----- IMG_7310.MOV
├──----- IMG_7311.MOV
├──----- IMG_7312.MOV
````
#### We have trained model till 160 epoch
- As our accuracy of our model is 81% people who want to replicate in between and can take it
  the higher accuracy feel free to download
- we are also providing the different stage of trained model so that people who want to consider
  it to use in transfer learning can also use this

- If you are retraining the model then specify path from the epoch h5 else give a base model path provided on github 


| Epochs        | Download Link           |
| -------------------|:-----------------------:|
| epoch 40  | [Gdrive link](https://drive.google.com/file/d/1TGdD2TmuE70tSdysma3Z1PZiMOxWPqaq/view?usp=sharing) |
| epoch 120 | [Gdrive link](https://drive.google.com/file/d/1tTcE1tfeg57QaHW3A9v2gd_mqme8rU3u/view?usp=sharing)     
| epoch 160 | [Gdrive link](https://drive.google.com/file/d/1JA_xsHkohFiX-T7vSGefpTGemqMbYDCt/view?usp=sharing) |
| base model | [Github link](https://github.com/matterport/Mask_RCNN/releases)|


**Custom Training Process**
```bash
python3 custom.py train --dataset=customImages --weights=mask_rcnn_damage_0160.h5 --logs logs/
```

**Background Training Process**
````bash
nohup python3 custom.py train --dataset=customImages --weights=mask_rcnn_damage_0160.h5 --logs logs/&
````
# Running Architecture for Application

![System Configration Image](https://raw.githubusercontent.com/Prady96/Pothole-Detection/master/Images/Pothole%20Detection%20Project.png?token=AHIVHIJUTMMMJIYCYLEPQN25D4EQC)

Explaination of each segment

For making the whole **product life cycle simple and economical** we have used
**two servers** and push the data in **1 bucket for easy access in the system accross**

This is connected with (Google Cloud Plateform) which is having 2 Virtual Machines

1. We have recieved the **input image from Mobile Application** that is stored in the server for every 24 Hours, Each application just uploads the data via API to server

We have not created any database, therefore we manage everything from file names and directory
When any image is clicked cordinates is added on the image name

image name looks like
````
[latitude]-[longitude].jpg
````
2. This is very small server which only collects data from all the application around we have also added compression in image to reduce the load on the server

Everyday at 9pm IST scheduled via cron script (server Two) would run and Mask RCNN
would run on all the images present in the working directory

3. Both these sever has shared drive, So all this data is fetched by another server this is high computation server this runs every 24 hour for 1 hour daily and process all the images periodically

image name looks like
````
[number_of_pothole]-[size_of_pothole]-[latitude]-[longitude].jpg
````

4. Final updated data is refelected on google maps which can be seen in the application

Now only names of the image is parsed in which we consider only two segments for color marking

and other two for the location to be marked on the google maps


# Download Andorid Application
We have used Apache Cordova Framework for the complete development of Android & iOS Application

| Andoird Application Link v0.5  | [Gdrive Link](https://drive.google.com/file/d/1YIfFLZkNAF3R810iWtj8L3ygyLK4RmVw/view?usp=sharing)          |
| -------------------|:-----------------------:|

## Working for the Andoird Application
![Wireframe of Application](https://raw.githubusercontent.com/Prady96/Pothole-Detection/master/Images/wireFrame_AndroidApp.jpg?token=AHIVHII2PZYI6KQPOLPYEGK5D4FJW)


**We have completed Application Development for Detectiono of Potholes on Roads**
_We have also open sourced are models for people who want to introduce tranfer learning or like to improve
this Deep learning model feel to take a pull requests_

**We have created Android Application for the detection of potholes on Roads by the end of february we will shifting our application and model to openSource.**

## Extra Utilities that we used while building this whole Project with Application

- [Shutil](https://docs.python.org/3/library/shutil.html)
- [gpustat](https://github.com/wookayin/gpustat)

## References used
- [Matterport MaskRCNN](https://github.com/matterport/Mask_RCNN)
- [Priya Diwani](https://www.analyticsvidhya.com/blog/2018/07/building-mask-r-cnn-model-detecting-damage-cars-python/)
- [CrowdAi](https://www.crowdai.org/)


_Feel free to contact me if you have any questions and/or suggestions _
**_[pradyumg@gmail.com](mailto:pradyumg@gmail.com)_**
