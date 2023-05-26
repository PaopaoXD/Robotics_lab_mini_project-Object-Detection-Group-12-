*Intro
Hello everyone, we are from group 12, In this video we are going to show you how to do object detection using YOLOv5 on jetson nano using a custom dataset we have annotate and trained ourselves, for our dataset, we have decided to detect weapons so let's get into the process

*Data Gathering process
So the first process is the data gathering process, since we are detecting weapons, we have gathered these pictures including all 3 types of weapons which will be annotated and trained using CVAT and Roboflow website the dataset here have around 126 pictures, but it will further increase after the augmentation process.

*Data annotation process
So Let's begin the annotation process, we are at the cvat website and imported all of our dataset,  we will create a bounding box that will label this knife, we adjust the size of the box to fit the knife as much as possible in order to increase the accuracy of the model. for example, if we have a picture of a man holding a gun , so we create a bounding box around the gun and label it gun!, we do this for every single picture in the dataset which took a while to complete.

After the annotation process is complete, it's time to save our annotations, we are going to export our annotations file to roboflow website for data augmentation and for splitting data. we will choose the export format a PASCAL VOC because it makes things a little bit easier when we import our data to roboflow, so here as you can see, we have our annotations folder ready to be used.!

*Data Preprocessing/Augmentation process

Next is the data augmentation and splitting process, so let's begin by creating a new project on roboflow and enter all of the information about the projects , then we upload our images in the dataset, along with the annotation files we annotated previously in CVAT. , so as you can see the website displays our annotations correctly, so after that we can now split our dataset into 3 main categories,test train and validation for my dataset size im going to go with 70 20 and 10

Once we've done that, we will preprocess that data first which includes orientation adjustment and resizing, we can actually add more if we wanted to but we're going to go with these two. then we can select the methods of data augmentation to optimize the performace while training, for our project we are going to choose these 8 methods here and generate their variations, so as you can see the dataset size increase to around 300 images of weapons.

After the generation is completed we will have the option to train the dataset here with roboflow which will automatically train it for us, or we can export the dataset and use the generated API key on the Google colab or jupyter notebook. So we are going to copy this link and use it on colab to train our dataset. 

*Training process in colab
So let's begin our training process, we're going to import all the libraries we need here, and define the of epocs that the model will train here we selected 70 because there are no significant improvement beyond that, 

Now we are going to import the dataset from roboflow by pasting the link that we copied earlier, then we define the classes name within our dataset and create the functions for training and plotting the bounding boxes while detecting an object

This is the code for creating the directory to save the results which we'll have to use later on,

then we are going to clone the yolov5 repository from github using git clone command which will create a directory in our runtime session.
after that we are going to install all of the libraries required using the requirment text file included in the yolov5 folder.

then its time to train the model, by calling the function created earlier, the model will be trained for the specified number of epochs which could take some time.

after the training process is complete, we are going to save the results in the directory we've specified, so here is the precision and recall scores for this training session and the best results is saved as best.pt in the runs folder, which we will need it for deployment.
and these below are just the codes for visualizing the training results it is based on image inferencing, but not real time.

*Deployment
next, it's time for deployment, first we have to boot up our jetson nano by connecting all the neccessary devices, flash an sd card with the downloaded images and set it up until we see this screen. Then, its ready

First, we open the terminal but before we do anything we will write sudo apt update to update the package lists for available software packages .we also need to install curl command so we will write sudo apt install curl to install the curl packages. 

The jetson nano is preinstalled with python3 version 3.6.9 which is not compatible with yoloV5 therefore, it is required that we upgrade our python version, here we decided to install another python3 version of 3.7.5 by running these code in the terminal, first we run this sudo apt get install build essential code, 
then we run sudo apt-get install python3-pip python 3.7 dev code
and we run sudo apt-get install python 3.7 to install python version 3.7.5

to change the current Python version, we have to run a code for updating alternatives version here

and as you can see when we check our python version it has been changed!

Then we will install the pip with the curl command earlier, inorder so that we can install the requirement libraries listed in the requirement text file. we will run the downloaded python file named get-pip.py to fully install pip.

and as you can see the pip 23.1.2 has been succesfully installed

next we will run the git clone command to clone the repository of yolov5 from git hub
then change the directory to that folder
as you can see a new folder has been created, we will put the train weights named best.pt that we got from colab during training into this folder.

and then installed every library we need with pip install requirements.txt 

as you can see everything we need is installed. including pytorch and torchvision

and we're set, the system is ready to detect weapons.

so let's begin the inferencing part, we are going to run the detection python file in the yolov5 folder by this code, python3 detect.py --source 0 --weights best.pt, as mentioned the best.pt file is our trained weights during the training process in the colab. 
So let's run and see what happens, as you can see the camera opens and it starts detecting.  so since I do not own any weapons, we will use pictures from the internet which may slightly affect the performance of the detection but everthing seems to work normally!.
