---
layout: post
title:      "CNN Sound Recognition Code Along"
date:       2021-01-23 18:31:43 -0500
permalink:  cnn_sound_recognition_code_along
---


![](https://poster.keepcalmandposters.com/1057195.png)

In this article, I would like to cover the basic idea of how to build a sound recognition Convolutional Neural Network Model. The basic concept is the following.

* Load Necessary Libraries
* Obtain Data
* Preprocess sound data into arrays 
* Build Model
* Check outcome

This notebook is designed with simple codes to cover the basic concept.

## 1. Load Necessary Libraries
Unlike image preprocessing from Keras, sound files can be processed with librosa, a python package for music and audio analysis. Check out [LIBROSA](https://librosa.org/doc/latest/index.html) documents for installation instruction and more information regarding the package.

```
# Data Processing
import pandas as pd
import glob
#Visualization
import matplotlib.pyplot as plt

#Scikit Learn Packages
from sklearn.model_selection import train_test_split #data split for training
from sklearn.metrics import confusion_matrix #evaluation of the model

# Audio preprocessing
import librosa

# Image Preprocessing
from keras.preprocessing.image import ImageDataGenerator

#Keras Packages
from tensorflow.keras.models import Sequential
from tensorflow.keras import layers

import numpy as np #utilitiy
```

## 2. Obtain Data

The data has been obtained from [Classifying Heart Sounds Challenges](http://www.peterjbentley.com/heartchallenge/) provided by Peter Bentley, Glenn Nordehn, Miguel Coimbra, Shie Mannor, Rita Getz and sponsored by [PASCAL](http://www.pascal-network.org/). Simple download from the website.

@misc{pascal-chsc-2011, author = "Bentley, P. and Nordehn, G. and Coimbra, M. and Mannor, S.", title = "The {PASCAL} {C}lassifying {H}eart {S}ounds {C}hallenge 2011 {(CHSC2011)} {R}esults", howpublished = "http://www.peterjbentley.com/heartchallenge/index.html"}

![](https://miro.medium.com/max/1528/0*J7faRuayovpnHnUp.png)

For this sample code along, I chose data set_b. Here is the cleaning process of the data.

```
# obtaining all existing file names
fname = pd.Series([name for name in glob.iglob("set_b/**")])

# file name contained the label, abstracting label from the file name
label = fname.str.replace("set_b/", "").str.split("_", expand=True)[0][:]

#make dataframe from obtained data for cleaning process
train_df = pd.DataFrame(fname, columns= ["fname"])
train_df["label"] = label

#getting rid of unlabelled data
train_df = train_df.loc[train_df.label != "Bunlabelledtest"]
```

## 3. Preprocess Sound Data into Arrays

In this process, we may extract important features like Spectral Centroid, Spectral Rolloff, Spectral Bandwidth, Zero-Crossing Rate, Mel-Frequency Cepstral Coefficients aka MFCCs, and Chroma feature from audio files. For this simple model, we will convert audio files into image files using Spectrogram. Then convert the image into NumPy arrays using Keras image preprocessing. 

Here is a sample code for extracting important features.

```
y, sr = librosa.load(train_df.fname.iloc[0])
rms = librosa.feature.rms(y=y)
chroma_stft = librosa.feature.chroma_stft(y=y, sr=sr)
spec_cent = librosa.feature.spectral_centroid(y=y, sr=sr)
spec_bw = librosa.feature.spectral_bandwidth(y=y, sr=sr)
rolloff = librosa.feature.spectral_rolloff(y=y, sr=sr)
zcr = librosa.feature.zero_crossing_rate(y)
mfcc = librosa.feature.mfcc(y=y, sr=sr)

```

To learn more about these features please visit this blog post by Pragati Baheti [Working with Audio Data for Machine Learning in Python](https://heartbeat.fritz.ai/working-with-audio-signals-in-python-6c2bd63b2daf).

![](https://upload.wikimedia.org/wikipedia/commons/c/c5/Spectrogram-19thC.png)

### Converting Audio File into Image File using Spectrogram

A Spectrogram is a visual representation of the spectrum of frequencies of a signal as it varies with time and holds many important information about the audio [Spectrogram by Wikipedia](https://en.wikipedia.org/wiki/Spectrogram).

Next we will convert the Audio File into the image file using libroas's feature.

```
cmap = plt.get_cmap('inferno')
plt.figure(figsize=(8,8))
for indx in range(num_f):
    filename = train_df.fname.iloc[indx]
    y, sr = librosa.load(filename, sr=sr, duration=5, res_type="kaiser_fast")
    dur = librosa.get_duration(y=y, sr=sr)
    if (round(dur) < 5):
        y = librosa.util.fix_length(y, sr*5)
    plt.specgram(y, NFFT=2048, Fs=2, Fc=0, noverlap=128, cmap=cmap, sides='default', mode='default', scale='dB');
    plt.axis('off');
    new_fname = filename[5:].replace(".wav",".png")
    plt.savefig('image/'+new_fname)
    plt.clf()
```

### Converting Image File into Array

In this step, we will convert the saved Spectrogram images into array.

First, we will need to obtain all image file names and make DataFrame.

#### Data Preparation
```
#Obtaining all image file names
imgname = pd.Series([name for name in glob.iglob("image/**")])[:-1]

#Abstrating labels as same method as above
imglabel = imgname.str.replace("image/", "").str.split("_", expand=True)[0][:]

# Coverting into a Dataframe
img_df = pd.DataFrame(imgname, columns= ["imgname"])
img_df['label'] = imglabel
```

#### Split Train, Test, Validation Set

To prepare model training, next we will split data into Train, Test, and Validation Set.
```
raw_X = img_df.drop('label', axis=1)
raw_y = img_df.label
train_X, val_X, train_y, val_y = train_test_split(raw_X, raw_y, test_size=.1,random_state=42)
train_X, test_X, train_y, test_y = train_test_split(train_X, train_y, test_size=.2, random_state=42)

#Prepare DataFrame for image preprocessing since we will be using flow from dataframe
train_df = train_X
train_df['label'] = train_y
test_df = test_X
test_df['label'] = test_y
val_df = val_X
val_df['label'] = val_y
```

#### Image Preprocessing using Keras ImageDataGenerator

In this process, we are coverting image files into arrays.

```
train_datagen = ImageDataGenerator(rescale=1./255).flow_from_dataframe(train_df,
                                                     x_col='imgname',
                                                     y_col='label',
                                                     target_size=(64, 64),
                                                     batch_size= train_size)
test_datagen = ImageDataGenerator(rescale=1./255).flow_from_dataframe(test_df,
                                                     x_col='imgname',
                                                     y_col='label',
                                                     target_size=(64, 64),
                                                     batch_size= test_size)
val_datagen =  ImageDataGenerator(rescale=1./255).flow_from_dataframe(val_df,
                                                     x_col='imgname',
                                                     y_col='label',
                                                     target_size=(64, 64),
                                                     batch_size= val_size)

train_images, train_labels = next(train_datagen)
test_images, test_labels = next(test_datagen)
val_images, val_labels = next(val_datagen)

```

## 4. CNN Model Building

For this, we will be using basic 4 layer Convolutional Neural Network model and train the machine.

```
model = Sequential()
model.add(layers.Conv2D(32, (3, 3), activation='relu',
                        input_shape=(64,64,3)))#1st Hidden Layer
model.add(layers.MaxPooling2D((2, 2))) #Maxpooling to 1st Layer

model.add(layers.Conv2D(32, (3, 3), activation='relu'))#2nd Hidden Layer
model.add(layers.MaxPooling2D((2, 2))) #Maxpooling to 2nd Layer

model.add(layers.Conv2D(64, (3, 3), activation='relu'))#3rd Hidden Layer
model.add(layers.MaxPooling2D((2, 2))) #Maxpooling to 3rd Layer

model.add(layers.Flatten()) #Flattening Layer
model.add(layers.Dense(64, activation='relu'))#4th Hidden Layer
model.add(layers.Dense(3, activation='sigmoid')) #Output Layer

model.compile(loss='binary_crossentropy',
              optimizer="adam", # For CNN model we will use 'adam' as an optimizer which is most commonly used optimizer
              metrics=['accuracy'])

history = model.fit(train_images, train_labels,
            epochs=150,
            batch_size=32,
            validation_data=(test_images, test_labels))
```

![](https://media3.giphy.com/media/l2JdYIOL79carDNiE/giphy.gif)
## 5. Outcome
It is now time to check the result of the model.

```
results_train = model.evaluate(train_images, train_labels)
print(f'Training Loss: {results_train[0]:.3} \nTraining Accuracy: {results_train[1]:.3}')

print('----------')

results_test = model.evaluate(val_images, val_labels)
print(f'Test Loss: {results_test[0]:.3} \nTest Accuracy: {results_test[1]:.3}')
```

The model exhibited the 100% accuracy for train and 78% accuracy for validation set. It was definitly over trained but not bad for the very basic model.

With further tuning of the model, I believe we can exhibit higher accuracy.

## Wrap up

In this blog, we have covered one of the methods to build CNN model for audio files. By converting audio files into image files using Spectrogram. Using converted image files to arrays using traditional Keras' image preprocessing. Then applied CNN model method to build model. WIth further tuning of the image files and medels, I believe higher accuracy can be obtained.

Thank you for reading this long blog and I hope this helps you in your journey.

![](https://discoveries.childrenshospital.org/wp-content/uploads/2019/11/Donor_ThankYou.jpg)
