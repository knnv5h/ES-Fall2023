# ES2023 - 實作9: AI起手式之圖像分類實作(2W)

## Lab 9-2 實作練習 & Python的5個回顧練習, 1W

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
