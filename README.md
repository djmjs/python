# python

# -*- coding: utf-8 -*-
"""Untitled1.ipynb

Automatically generated by Colaboratory.

Original file is located at
    https://colab.research.google.com/drive/1nmthSlvddq79fWIiovCTuu8OCKQHmwR2
"""

import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

dataset_mnist= tf.keras.datasets.fashion_mnist

(train_images, train_labels), (test_images, test_labels)= dataset_mnist.load_data()

class_names=["t-shirt","trouser","T-shirt/top", "Trouser", "Pullover", "Dress", "Coat",
               "Sandal", "Shirt", "Sneaker", "Bag", "Ankle boot"]
print(train_images.shape)
print(int(len(train_labels)))
print(list(train_labels)[:10])
print(test_images.shape)
print(int(len(test_labels)))

plt.figure()
plt.imshow(train_images[16120])
plt.hot()
plt.grid(False)
plt.show()

train_images= train_images/255.0
test_images = test_images/255.0

plt.figure(figsize=(10,10))
for i in range(25):
  plt.subplot(5,5,i+1)
  plt.xticks([])
  plt.yticks([])
  plt.imshow(train_images[i], cmap=plt.cm.binary)
  plt.xlabel(class_names[train_labels[i]])
plt.show()

model= tf.keras.Sequential([
  tf.keras.layers.Flatten(input_shape=(28,28)),
  tf.keras.layers.Dense(128,activation='relu'),
  tf.keras.layers.Dense(10,activation='softmax')
])
model.summary()

model.compile(
  optimizer='adam',
  loss=tf.keras.losses.SparseCategoricalCrossentropy(),
  metrics =['accuracy'])

model.fit(train_images,train_labels, epochs=5)


test_loss, test_acc = model.evaluate(test_images,  test_labels, verbose=2)

print('\nTest accuracy:', test_acc)