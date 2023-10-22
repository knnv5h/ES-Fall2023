# ES2023 - 實作9: AI起手式之圖像分類實作(2W)
## 目錄
[實作1](https://github.com/knnv5h/ES-Fall2023/blob/main/Lab9/Lab9-2.md#https://github.com/knnv5h/ES-Fall2023/blob/main/Lab9/Lab9-2.md#實作1-從已提供的選項中找1張自己喜歡的照片來試試看)
[實作2](https://github.com/knnv5h/ES-Fall2023/blob/main/Lab9/Lab9-2.md#實作2-從網路上找3張自己喜歡的照片來試試看-jpgpng)
## Lab 9-2 實作練習 & Python的5個回顧練習, 1W

### 實作1: 從已提供的選項中,找1張自己喜歡的照片來試試看

#### 輸出

![未命名](https://github.com/knnv5h/ES-Fall2023/assets/43922704/0310a2ee-0bbe-44db-892d-445f3b168b6d)

#### 程式
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


## 804 選項: ['tiger', 'bus', 'car', 'cat', 'dog', 'apple', 'turtle', 'flamingo', 'piano', 'honeycomb', 'teapot']
"""## 3. 選擇圖像: 您可以選擇以下圖像之一，或使用您自己的圖像。 請記住，模型的輸入大小各不相同，其中一些使用動態輸入大小（啟用對未縮放圖像的推斷）。 鑑於此，方法 load_image 已經將圖像重新縮放為預期格式。

選項: ['老虎'，'公共汽車'，'汽車'，'貓'，'狗'，'蘋果'，'烏龜'，'火烈鳥'，'鋼琴'，'蜂窩'，'茶壺']
param ['tiger', 'bus', 'car', 'cat', 'dog', 'apple', 'turtle', 'flamingo', 'piano', 'honeycomb', 'teapot','cy']
"""



image_name = 'turtle' 

images_for_test_map = {
    "tiger": "https://upload.wikimedia.org/wikipedia/commons/b/b0/Bengal_tiger_%28Panthera_tigris_tigris%29_female_3_crop.jpg",
    "bus": "https://upload.wikimedia.org/wikipedia/commons/6/63/LT_471_%28LTZ_1471%29_Arriva_London_New_Routemaster_%2819522859218%29.jpg",
    "car": "https://upload.wikimedia.org/wikipedia/commons/4/49/2013-2016_Toyota_Corolla_%28ZRE172R%29_SX_sedan_%282018-09-17%29_01.jpg",
    "cat": "https://upload.wikimedia.org/wikipedia/commons/4/4d/Cat_November_2010-1a.jpg",
    "dog": "https://upload.wikimedia.org/wikipedia/commons/archive/a/a9/20090914031557%21Saluki_dog_breed.jpg",
    "apple": "https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg",
    "turtle": "https://upload.wikimedia.org/wikipedia/commons/8/80/Turtle_golfina_escobilla_oaxaca_mexico_claudio_giovenzana_2010.jpg",
    "flamingo": "https://upload.wikimedia.org/wikipedia/commons/b/b8/James_Flamingos_MC.jpg",
    "piano": "https://upload.wikimedia.org/wikipedia/commons/d/da/Steinway_%26_Sons_upright_piano%2C_model_K-132%2C_manufactured_at_Steinway%27s_factory_in_Hamburg%2C_Germany.png",
    "honeycomb": "https://upload.wikimedia.org/wikipedia/commons/f/f7/Honey_comb.jpg",
    "teapot": "https://upload.wikimedia.org/wikipedia/commons/4/44/Black_tea_pot_cropped.jpg",
    "cy":"https://hinetcdn.waca.ec/uploads/shops/2303/products/18/18e3142acb70acff1c4dc8ce8b635669.jpg"
}

img_url = images_for_test_map[image_name]
image, original_image = load_image(img_url, image_size, dynamic_size, max_dynamic_size)

#實作1: 從已提供的選項中,找1張自己喜歡的照片來試試看


# %time 
classifier = hub.load(model_handle)
probabilities = tf.nn.softmax(classifier(image)).numpy()

top_5 = tf.argsort(probabilities, axis=-1, direction="DESCENDING")[0][:5].numpy()
np_classes = np.array(classes)
includes_background_class = probabilities.shape[1] == 1001
E_2_TW = Translator(to_lang="zh-TW") # 英翻中

for i, item in enumerate(top_5):
  class_index = item if not includes_background_class else item - 1
  line = f'({i+1}) {class_index:4} - {classes[class_index]}: {probabilities[0][top_5][i]}'
  translation1 = E_2_TW.translate(classes[class_index])
  print(line, ', ', translation1)

show_image(image, '')
```
### 實作2: 從網路上找3張自己喜歡的照片來試試看 (jpg/png)

#### 輸出

![1](https://github.com/knnv5h/ES-Fall2023/assets/43922704/7e120484-441b-49a0-b754-4179d94f672c)
![2](https://github.com/knnv5h/ES-Fall2023/assets/43922704/aa78bd2c-86fa-41c0-9768-7e1801d31757)
![3](https://github.com/knnv5h/ES-Fall2023/assets/43922704/645bad70-bbe8-4777-904b-ecbe2d1a7642)

#### 程式
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


## 804 選項: ['tiger', 'bus', 'car', 'cat', 'dog', 'apple', 'turtle', 'flamingo', 'piano', 'honeycomb', 'teapot']
"""## 3. 選擇圖像: 您可以選擇以下圖像之一，或使用您自己的圖像。 請記住，模型的輸入大小各不相同，其中一些使用動態輸入大小（啟用對未縮放圖像的推斷）。 鑑於此，方法 load_image 已經將圖像重新縮放為預期格式。

選項: ['老虎'，'公共汽車'，'汽車'，'貓'，'狗'，'蘋果'，'烏龜'，'火烈鳥'，'鋼琴'，'蜂窩'，'茶壺']
param ['tiger', 'bus', 'car', 'cat', 'dog', 'apple', 'turtle', 'flamingo', 'piano', 'honeycomb', 'teapot','cy']
"""



image_name = 'pizza'

images_for_test_map = {
    "tiger": "https://upload.wikimedia.org/wikipedia/commons/b/b0/Bengal_tiger_%28Panthera_tigris_tigris%29_female_3_crop.jpg",
    "bus": "https://upload.wikimedia.org/wikipedia/commons/6/63/LT_471_%28LTZ_1471%29_Arriva_London_New_Routemaster_%2819522859218%29.jpg",
    "car": "https://upload.wikimedia.org/wikipedia/commons/4/49/2013-2016_Toyota_Corolla_%28ZRE172R%29_SX_sedan_%282018-09-17%29_01.jpg",
    "cat": "https://upload.wikimedia.org/wikipedia/commons/4/4d/Cat_November_2010-1a.jpg",
    "dog": "https://upload.wikimedia.org/wikipedia/commons/archive/a/a9/20090914031557%21Saluki_dog_breed.jpg",
    "apple": "https://upload.wikimedia.org/wikipedia/commons/1/15/Red_Apple.jpg",
    "turtle": "https://upload.wikimedia.org/wikipedia/commons/8/80/Turtle_golfina_escobilla_oaxaca_mexico_claudio_giovenzana_2010.jpg",
    "flamingo": "https://upload.wikimedia.org/wikipedia/commons/b/b8/James_Flamingos_MC.jpg",
    "piano": "https://upload.wikimedia.org/wikipedia/commons/d/da/Steinway_%26_Sons_upright_piano%2C_model_K-132%2C_manufactured_at_Steinway%27s_factory_in_Hamburg%2C_Germany.png",
    "honeycomb": "https://upload.wikimedia.org/wikipedia/commons/f/f7/Honey_comb.jpg",
    "teapot": "https://upload.wikimedia.org/wikipedia/commons/4/44/Black_tea_pot_cropped.jpg",
    "cy":"https://hinetcdn.waca.ec/uploads/shops/2303/products/18/18e3142acb70acff1c4dc8ce8b635669.jpg",
    "candle":"https://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Candles_%28Christmas_2009%29.jpg/220px-Candles_%28Christmas_2009%29.jpg",
    "Screw":"https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Screw.jpg/260px-Screw.jpg",
    "pizza":"https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Kartoffel_pizza_%28with_border%29.jpg/221px-Kartoffel_pizza_%28with_border%29.jpg"
}

img_url = images_for_test_map[image_name]
image, original_image = load_image(img_url, image_size, dynamic_size, max_dynamic_size)

#實作2: 從網路上找3張自己喜歡的照片來試試看 (jpg/png)
# %time
classifier = hub.load(model_handle)
probabilities = tf.nn.softmax(classifier(image)).numpy()

top_5 = tf.argsort(probabilities, axis=-1, direction="DESCENDING")[0][:5].numpy()
np_classes = np.array(classes)
includes_background_class = probabilities.shape[1] == 1001
E_2_TW = Translator(to_lang="zh-TW") # 英翻中

for i, item in enumerate(top_5):
  class_index = item if not includes_background_class else item - 1
  line = f'({i+1}) {class_index:4} - {classes[class_index]}: {probabilities[0][top_5][i]}'
  translation1 = E_2_TW.translate(classes[class_index])
  print(line, ', ', translation1)

show_image(image, '')
```
### Python的5個回顧練習

#### 輸出

![螢幕擷取畫面 2023-10-21 212921](https://github.com/knnv5h/ES-Fall2023/assets/43922704/8111e3d1-10cb-42a6-8647-a3b9a77ff5f2)

#### 程式
``` python
for i in range(1, 10):
    print('*' * i)
```
#### 輸出

![螢幕擷取畫面 2023-10-21 213410](https://github.com/knnv5h/ES-Fall2023/assets/43922704/7b95b1e2-854a-4adb-8d42-1447fe079d43)

#### 程式
``` python
for i in range(9,29,2):
    print(2+i)
```
#### 輸出

![螢幕擷取畫面 2023-10-21 213548](https://github.com/knnv5h/ES-Fall2023/assets/43922704/4c905f78-e10e-435f-9720-bee0a5d259b5)

#### 程式
``` python
for i in range(32,4,-2):
    print(i-2)
```

#### 輸出

![螢幕擷取畫面 2023-10-21 213820](https://github.com/knnv5h/ES-Fall2023/assets/43922704/c83d3b17-429e-44f8-85dc-e3fb4d179e70)

#### 程式
``` python
for i in range(1, 4):  # 外層迴圈控制第一個數字（1到3）
    for j in range(1, 4):  # 內層迴圈控制第二個數字（1到3）
        result = i * j  # 計算乘法結果
        print(f"{i} X {j} = {result}")  # 印出乘法表達式和結果
```

#### 輸出

![螢幕擷取畫面 2023-10-21 214333](https://github.com/knnv5h/ES-Fall2023/assets/43922704/327f9c92-05e2-4318-a9d8-a82bad69e1ed)

#### 程式
``` python
def even_numbers(n):
    # 使用列表生成式生成被2整除的正整數清單
    even_list = [num for num in range(2, n+1) if num % 2 == 0]
    return even_list

# 讓使用者輸入數字
n = int(input("請輸入一個正整數："))

# 呼叫函式並印出結果
result = even_numbers(n)
print(result)
```
