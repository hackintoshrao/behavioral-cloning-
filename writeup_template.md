#**Behavioral Cloning**

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/placeholder.png "Model Visualization"
[image2]: ./examples/placeholder.png "Grayscaling"
[image3]: ./examples/placeholder_small.png "Recovery Image"
[image4]: ./examples/placeholder_small.png "Recovery Image"
[image5]: ./examples/placeholder_small.png "Recovery Image"
[image6]: ./examples/placeholder_small.png "Normal Image"
[image7]: ./examples/placeholder_small.png "Flipped Image"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network
* writeup_report.md or writeup_report.pdf summarizing the results

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing
```sh
python drive.py model.h5
```

####3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

###Model Architecture and Training Strategy


####1. Attempts to reduce overfitting in the model

- The model contains 3 dropout layers in order to reduce overfitting, the architecture table above describes it well. Also the data is initially shuffled before splitting for validation.

- Initially with a very simple 2 layered model, the model was underfitting. On incrementally increasing the complexity of the model the error on the training data reduced, but the score on the validation was still low due to overfitting. Once the dropout layers were added the score on the validation data set turned out to be great. The model was trained and validated on different data sets to ensure that the model was not overfitting. The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

####2. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually.

####3. Appropriate training data

One of the center/right/left image was randomly chosen each time and the flipped image was added.


###Model Architecture and Training Strategy

####1. Solution Design Approach

The overall strategy for deriving a model architecture was to start with simple model and incrementally increase the complexity by adding more layers till a good accuracy on the test set was obtained.
Based on the score on the validation set dropout layers were added to reduce overfitting later.


At the end of the process, the vehicle was able to drive autonomously around the track without leaving the road.

####2. Final Model Architecture

Here is the model architecture table
| Layer                         |     Description
|:---------------------:|:---------------------------------------------:|
| Input                 |       | 64x64x3 RGB image
| Convolution           |       | 5x5 filters, default stride, default padding
| Activation            |       | ELU
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| Convolution           |       | 3 x 3 filters, default stride, default padding
| Activation            |       | ELU
| Dropout               |       | 0.25
| Max pooling           |       | 2x2 stride
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| Convolution           |       | 3x3 filters, default stride, default padding
| Activation            |       | ELU
| Dropout               |       | 0.25
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| Flatten               |       |
| Fully connected       |       | output 1024
| Activation            |       | ELU
| Dropout               |       | 0.3
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| Fully connected       |       | output 1024 -> 256
| Activation            |       | ELU
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
| Fully connected       |       | output 256 -> 1
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

- The model includes ELU layers to introduce nonlinearity.
- The data is normalized and preproceesed using the  `process_get_data` function.

  ```
  def image_preprocessing(image):
   """
   Crop the image, resize and normalize.
   """
   image = image[50:130, :, :]
   image = cv2.resize(image, (64, 64))
   image = image.astype(np.float32)
   image = image/255.0 - 0.5
   return image
  ```


####3. Creation of the Training Set & Training Process

- Used the training set provided by Udacity.
- Used 20% of it for validation.
- Randomly chose one of the right/left/center image and used the flipped image too.
- Used 22000 samples from generator for training each epoch.
- The model is trained for a total of 3 epochs.