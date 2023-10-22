# ES2023 - 實作9: AI起手式之圖像分類實作(2W)

## Lab 9-1 AI應用起手式: 使用 TensorFlow Hub 進行圖像分類, 1W


### 輸出
![未命名](https://github.com/knnv5h/ES-Fall2023/assets/43922704/aee3f067-7752-4ad8-be3f-82a3ade98dd2)

### 程式
``` python
# 801 1. 載入必要的Module
import tensorflow as tf
import tensorflow_hub as hub

import requests
from PIL import Image
from io import BytesIO
!pip install translate
from translate import Translator
import matplotlib.pyplot as plt
import numpy as np
from datetime import date
today = date.today()
ts3 = 'Leo'

# 802 載入必要的Function
original_image_cache = {}

def preprocess_image(image):
  image = np.array(image)
  # reshape into shape [batch_size, height, width, num_channels]
  img_reshaped = tf.reshape(image, [1, image.shape[0], image.shape[1], image.shape[2]])
  # Use `convert_image_dtype` to convert to floats in the [0,1] range.
  image = tf.image.convert_image_dtype(img_reshaped, tf.float32)
  return image

def load_image_from_url(url):
  """Returns an image with shape [1, height, width, num_channels]."""
  user_agent = {'User-agent': 'Colab Sample (https://tensorflow.org)'}
  response = requests.get(img_url, headers=user_agent)
  image = Image.open(BytesIO(response.content))
  image = preprocess_image(image)
  return image

def load_image(image_url, image_size=256, dynamic_size=False, max_dynamic_size=512):
  """Loads and preprocesses images."""
  # Cache image file locally.
  if image_url in original_image_cache:
    img = original_image_cache[image_url]
  elif image_url.startswith('https://'):
    img = load_image_from_url(image_url)
  else:
    fd = tf.io.gfile.GFile(image_url, 'rb')
    img = preprocess_image(Image.open(fd))
  original_image_cache[image_url] = img
  # Load and convert to float32 numpy array, add batch dimension, and normalize to range [0, 1].
  img_raw = img
  if tf.reduce_max(img) > 1.0:
    img = img / 255.
  if len(img.shape) == 3:
    img = tf.stack([img, img, img], axis=-1)
  if not dynamic_size:
    img = tf.image.resize_with_pad(img, image_size, image_size)
  elif img.shape[1] > max_dynamic_size or img.shape[2] > max_dynamic_size:
    img = tf.image.resize_with_pad(img, max_dynamic_size, max_dynamic_size)
  return img, img_raw

def show_image(image, title=''):
  image_size = image.shape[1]
  w = (image_size * 6) // 320
  plt.figure(figsize=(w, w))
  plt.imshow(image[0], aspect='equal')
  plt.axis('off')
  plt.title(title)
  plt.show()

## 803, Select an Image Classification model

image_size = 224
dynamic_size = False

model_name = "efficientnetv2-s"

model_handle_map = {"efficientnetv2-s": "https://tfhub.dev/google/imagenet/efficientnet_v2_imagenet1k_s/classification/2"}

model_image_size_map = {"efficientnetv2-s": 384}

model_handle = model_handle_map[model_name]

print(f"Selected model: {model_name} : {model_handle}")

max_dynamic_size = 512
if model_name in model_image_size_map:
  image_size = model_image_size_map[model_name]
  dynamic_size = False
  print(f"Images will be converted to {image_size}x{image_size}")
else:
  dynamic_size = True
  print(f"Images will be capped to a max size of {max_dynamic_size}x{max_dynamic_size}")

labels_file = "https://storage.googleapis.com/download.tensorflow.org/data/ImageNetLabels.txt"

#download labels and creates a maps
downloaded_file = tf.keras.utils.get_file("labels.txt", origin=labels_file)

classes = []
i = 0
with open(downloaded_file) as f:
  labels = f.readlines()
  classes = [l.strip() for l in labels[1:]]
  i += 1

# 809 實作B: 在圖像上運行模型
# 
%time 

probabilities = tf.nn.softmax(classifier(image)).numpy()

top_5 = tf.argsort(probabilities, axis=-1, direction="DESCENDING")[0][:5].numpy()
np_classes = np.array(classes)
includes_background_class = probabilities.shape[1] == 1001
print(len(top_5), top_5)
E_2_TW = Translator(to_lang="zh-TW") # 英翻中
for i, item in enumerate(top_5):
  
  class_index = item if not includes_background_class else item - 1
  
  line = f'({i+1}) {class_index:4} - {classes[class_index]}: {probabilities[0][top_5][i]}'
  animal = classes[class_index]
  translation1 = classes[class_index] #E_2_TW.translate(classes[class_index])
  
  print(line, ', ', E_2_TW.translate(animal),',', translation1)

show_image(image, '')


from datetime import datetime
today = datetime.now()
print('*** Done by %s at ' % ts3,today, type(today))
```
