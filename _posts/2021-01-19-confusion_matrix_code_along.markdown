---
layout: post
title:      "Confusion Matrix Code Along"
date:       2021-01-19 06:50:48 +0000
permalink:  confusion_matrix_code_along
---

In case you have questions regarding the confusion matrix, please visit my previous blog for more information. [How To Interpret The Confusion Matrix](https://swk426.github.io/how_to_interpret_the_confusion_matrix).

## Part 2.
![](https://memegenerator.net/img/instances/59317095.jpg)


In this blog, we will go over how to apply the Confusion Matrix in python. To help the understanding, I will be building a simple Neural Network model to build a model and perform a confusion matrix in python. The basic outline of the article is following.

1. Import Necessary Libraries
2. Data Obtaining
3. Data Cleaning
4. Build a Neural Network Model
5. Perform Confusion Matrix

The Data is obtained from Paul Mooney's [Chest X-Ray Images (Pneumonia)](https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia)

![](https://i.imgur.com/8AUJkin.png)

## 1. Import Necessary Libraries
We will start by importing necessary librarires.

```
import pandas as pd #Data Processing and Organizing
import matplotlib.pyplot as plt #For Visual
%matplotlib inline

import numpy as np # For Array Interpreting
from datetime import datetime # Measures Time for Model running

from tensorflow.keras import models # Model for Neural Network
from tensorflow.keras import layers # Layers for the model
from keras.preprocessing.image import ImageDataGenerator, array_to_img,\
                                      img_to_array, load_img #image preprocessing

from sklearn.metrics import confusion_matrix # Confusion Matrix Processing
```

## 2. Data Obtaining
Data obtain was rather simple. It was downloaded straight from the Kaggle site. The data is the x-ray pictures of the children's chest in jpeg form. It was originally splitted into train set (5216 images), validation set (16 images) and test set (624 images). After running the model few times, to bring better validation results in model, I have moved about 400 images from train set to validation set.
We will set up the Directory path of the data.

```
# Directory path
train_data_dir = 'chest_xray/train'
test_data_dir = 'chest_xray/test'
val_data_dir = 'chest_xray/val'
```

## 3. Data Cleaning
For image recognition, data cleaning is preprocessing JPEG format of data into machine recognizable arrays.

We will use Keras Image preprocessing. Please visit [Image data preprocessing
](https://keras.io/api/preprocessing/image/) for more information.

```
# Get all the data in the directory data/train (1204 images - 1/4 of the images), normalize and reshape them
train_generator = ImageDataGenerator(rescale=1./255).flow_from_directory(
                                            train_data_dir, 
                                            target_size=(32, 32), #size of the image
                                            batch_size=1204) #number of images to train

# Get all the data in the directory data/validation (624 images- 1/4 of the images), normalize and reshape them
test_generator = ImageDataGenerator(rescale=1./255,).flow_from_directory(
                                            test_data_dir, 
                                            target_size=(32, 32),
                                            batch_size=156)

# get all the data in the directory split/validation (416 images- 1/4 of the images), normalize and reshape them
val_generator = ImageDataGenerator(rescale=1./255).flow_from_directory(
                                            val_data_dir,
                                            target_size=(32,32),
                                            batch_size=104)

# Create the datasets
train_images, train_labels = next(train_generator)
test_images, test_labels = next(test_generator)
val_images, val_labels = next(val_generator)

# Reshape images and labels for Neural Network to Recognize the array.
train_img = train_images.reshape(train_images.shape[0], -1)
test_img = test_images.reshape(test_images.shape[0], -1)
val_img = val_images.reshape(val_images.shape[0], -1)

train_y = np.reshape(train_labels[:,0], (train_img.shape[0],1))
test_y = np.reshape(test_labels[:,0], (test_img.shape[0],1))
val_y = np.reshape(val_labels[:,0], (val_img.shape[0],1))
```

## 4. Neural Network Model
We will build a simple 3 layered Nerual Network Model and see how it performs.

```
start_time=datetime.now()# To check how long each model run
basic_model = models.Sequential()
basic_model.add(layers.Dense(20, activation='relu', input_shape=(train_img.shape[1],))) #1st hidden layers
basic_model.add(layers.Dense(7, activation='relu'))#2nd hidden layers
basic_model.add(layers.Dense(5, activation='relu'))#3rd hidden layers
basic_model.add(layers.Dense(1, activation='sigmoid'))#output layer
basic_model.compile(optimizer='sgd',
              loss='binary_crossentropy',
              metrics=['accuracy']) #commonly used parameters
basic_history = basic_model.fit(train_img,
                    train_y,
                    epochs=150,
                    batch_size=32,
                    validation_data=(test_img, test_y))
end_time = datetime.now()
print('Time elapsed', end_time - start_time)
```

![](https://thumbs.gfycat.com/VariableFirsthandGoldenmantledgroundsquirrel-small.gif)

## 5. Confusion Matrix
Finally, you have been waiting for how to use confusion matrix. Here is a little break down of the steps.

1. Have the model perform predictions.
2. Use confusion matrix function from sklearn. Here is the sklearn metrics doc   [sklearn.metrics.confusion_matrix
](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html).
3. Visualize the result. You can use heat map and data frame.

```
#Perform predictions
basic_predictions = basic_model.predict_classes(val_img)

# Confusion Matrix Function from Sklearn. 
cm = confusion_matrix(val_y, basic_predictions, labels=[0,1],)
# During reshape of the labels, labels switched. 0 means Pnuemonia, 1 means Normal.

#Visualize, I like simple visual using Pandas Dataframe
index=["Actual Pnuemonia", "Actual Normal"]
columns=["Predicted Pnuemonia", "Predicted Normal"]
cm_df = pd.DataFrame(data=cm,index=index, columns=columns)
cm_df
```

For the quick model, my basic model performed following result.

| | Predicted Pnuemonia | Predicted Normal|
|------|------|------|
| Actual Pneumonia | 44 | 2 |
| Actual Normal | 4 | 54 |

From here, I can make interpret my model and work through the parameters to bring better results that meets the satisfaction. 

## Wrap Up

I hope this code along helped you with the general idea how to perform confusion matrix in python. Now let's go conquere that code.

![](https://i.pinimg.com/originals/96/c9/d8/96c9d878a4491b624e26bc78ff169349.jpg)

