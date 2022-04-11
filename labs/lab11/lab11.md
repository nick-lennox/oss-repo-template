# Lab 11

## Checkpoint 1

<img src="https://i.imgur.com/6vPBhkK.png" width=800>

## Checkpoint 2

<img src="https://i.imgur.com/FzeCGFU.png" width=800>

## Checkpoint 3

#### Images (original)

<img src="https://i.imgur.com/LsBfizX.jpeg" width=500>
<img src="https://i.imgur.com/qbM3NMv.jpeg" width=500>
<img src="https://i.imgur.com/LI8s67I.jpeg" width=500>

#### Images (greyscale)

<img src="https://i.imgur.com/pq0LsHy.png">
<img src="https://i.imgur.com/t5QXso0.png">
<img src="https://i.imgur.com/leH9N86.png">

#### Code

```
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
from PIL import ImageOps
 
fashion_mnist = tf.keras.datasets.fashion_mnist
 
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()
 
class_names = ['T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat',
               'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']
 
prepared_images = []
for filename in ['Untitled-1.png', 'Untitled-2.png', 'Untitled-3.png']:
  image = Image.open(filename)
  image = ImageOps.grayscale(image)
  prepared_images.append(np.asarray(image))
  
test_images = np.array(prepared_images)
test_images = test_images / 255.0
train_images = train_images / 255.0
 
model = tf.keras.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(10)
])
 
model.compile(optimizer='adam',
              loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
              metrics=['accuracy'])
 
model.fit(train_images, train_labels, epochs=10)
 
predictions = model.predict(test_images)
 
for prediction in predictions:
  print(prediction, np.argmax(prediction[0]), class_names[np.argmax(prediction[0])])
```

#### Output

```
[  1.7529287  -5.4313693  -3.1236343 -26.0195    -33.958996  -27.081797
   6.1699815 -56.81471    26.647861  -31.586836 ] 0 T-shirt/top
[ -6.8740077 -18.412466  -14.169606  -53.200195  -52.390278  -11.702952
  -4.0409737 -41.106983   20.632673  -11.468171 ] 0 T-shirt/top
[ -2.9555128 -15.4606     -8.011112  -53.600212  -37.4586    -14.492147
   7.998797  -42.785736   30.269007  -15.0284605] 0 T-shirt/top
 ```
 
2 out of the 3 were correct. Out of the three I would've guessed the pants would be more likely to be incorrect, but overall it's not bad!
