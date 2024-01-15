> 	I downloaded a model that performs categorical classification on images. I want to use this model in a web application, but it doesn't seem to be very accurate. Can you check out the weights and see if you can figure out what's wrong?
> 
> Author: windex
> 
> [http://35.209.84.6/](http://35.209.84.6/)

We are given the architecture to a image classification neural network, and the ability to upload a zipped folder of 28x28 images. Once uploaded, the user is given the 10-D output vector for each uploaded image. 

Using this (and the hint), we are supposed to replicate the model architecture and it's weights, and presumably upload the keras weights file to get the flag. 

I used this paper as direction to try to steal the model weights since the authors also used 28x28 images: https://arxiv.org/abs/1912.08987

My first step was to recreate the model in Keras (without weights):

```
import os
import numpy as np
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Conv2D, MaxPooling2D, Dropout, Flatten
from tensorflow.keras.preprocessing.image import load_img, img_to_array
from tensorflow.keras.optimizers import Adam
import numpy as np
from PIL import Image

def load_and_preprocess_images(folder_path, image_size=(28, 28)):
    images = []
    for file in sorted(os.listdir(folder_path)):
        if file.endswith('.png'):
            img_path = os.path.join(folder_path, file)
            img = load_img(img_path, color_mode='grayscale', target_size=image_size)
            img_array = img_to_array(img)
            img_array = img_array / 255.0 
            images.append(img_array)
    return np.array(images)
  

def build_model():
    model = Sequential()
    model.add(Conv2D(2, (3, 3), activation='relu', input_shape=(28, 28, 1)))
    model.add(MaxPooling2D((2, 2)))
    model.add(Flatten())
    model.add(Dense(4, activation='relu'))
    model.add(Dropout(0.25))
    model.add(Dense(10, activation='softmax')) 
    model.compile(optimizer=Adam(), loss='categorical_crossentropy', metrics=['accuracy'])
    return model

def load_preds(preds_file):
    with open(preds_file, "r+") as file:
        content = file.read()
        predictions = eval(content)
        return np.array(predictions)

folder_path = './images'  
images = load_and_preprocess_images(folder_path)
preds = load_preds("./responses.txt")

model = build_model()
model.fit(images, preds, epochs=50, batch_size=10)
model.save('surrogate_model.h5')
```

After that, all we need is lots of 28x28 png images and their corresponding predictions (stored in responses.txt). At first, I tried generating uniformly random greyscale images, but that didn't seem to work. Then, I used random images generated from a Bernoulli distribution, as suggested by the authors of the paper I was copying, but again, my model weights were not being accepted by the checker. 

Finally, I realized that the inputs were 28x28, just like the most famous ML dataset of all time: MNIST. I found a large png MNIST dataset online with ~600,000 images: https://github.com/myleott/mnist_png, zipped them all, and trained the model on the given predictions. Miraculously, this time the checker accepted the model weights, giving us the flag: UofTCTF{1t_w4s_ju5t_mn1st_101}