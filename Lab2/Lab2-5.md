# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-5: 按下按鍵, Green LED亮 & Red LED滅; 放開按鍵, Green LED滅 & Red LED亮. 想要再深入的同學可以試試喔. (思考方向: digitalRead(), digitalWrite(): 按鍵 +序列輸出 + LED), (互動5) (2W)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/57f5abdd-ee55-447d-8768-7138a032bd6e

### 程式
```C
// 定義按鈕和LED的腳位
const int buttonPin = 2;  // 按鈕接腳
const int ledPins[] = {7, 8, 13};  // LED 接腳（可以有多個LED）

// 變數來追蹤按鈕狀態和目前亮著的LED的索引
int buttonState = 0;
int lastButtonState = 0;
int currentLed = 0;

void setup() {
  // 設定按鈕接腳為輸入
  pinMode(buttonPin, INPUT);
  
  // 設定LED接腳為輸出
  for (int i = 0; i < 3; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
}

void loop() {
  // 讀取按鈕狀態
  buttonState = digitalRead(buttonPin);

  // 檢查按鈕是否被按下（偵測到上升沿）
  if (buttonState == HIGH && lastButtonState == LOW) {
    // 關閉目前亮著的LED
    digitalWrite(ledPins[currentLed], LOW);
    
    // 將currentLed移至下一個LED（形成循環）
    currentLed = (currentLed + 1) % 3;

    // 開啟下一個LED
    digitalWrite(ledPins[currentLed], HIGH);
  }

  // 更新按鈕狀態
  lastButtonState = buttonState;
}

```
