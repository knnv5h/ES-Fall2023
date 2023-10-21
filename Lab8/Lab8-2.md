# ES2023 - 實作8: 零基礎Python快速入門與實作 (2W) 

## Lab 8-2 零基礎Python快速入門與實作, 2/2, W14

### 輸出

![螢幕擷取畫面 2023-10-21 203858](https://github.com/knnv5h/ES-Fall2023/assets/43922704/dcec24c9-ae68-43f8-9942-7c05f9c7dbca)

### 程式
``` python
ts3 = 'Leo' # Please input your English name
import numpy as np
import matplotlib.pyplot as plt

### 實作1185
from math import pi
x = np.arange(0, 2 * np.pi, 0.01)
y_sin = np.sin(x)
y_cos = np.cos(x)

y_sin2 = np.sin(x+pi)
y_cos2 = np.cos(x+pi)

plt.subplots_adjust( left=0.1, right=1.5, top=1.5, bottom=0.1, wspace=0.2, hspace=0.2)

plt.subplot(221)
plt.plot(x, y_sin, color ='r') # red
plt.title('sin_red by Grace')

plt.subplot(222)
plt.plot(x, y_cos, color ='g') # green
plt.title('cos_green by Grac')

plt.subplot(223)
plt.plot(x, y_sin2, color ='y') # yellow
plt.title('sin_yellow by Grace')

plt.subplot(224)
plt.plot(x, y_cos2, color ='b') # blue
plt.title('cos_blue by Grace')

plt.show()

### Final Result
from datetime import datetime
today = datetime.now()
print('*** Done by %s at ' % ts3,today, type(today))
```
