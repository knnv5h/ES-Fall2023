# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-3, 讓你的RGB LED燈全彩模組也可會"呼吸", LED顏色變化是否有像"呼吸的效果"和示波器的波形有什麼關連性? (互動3), (1W)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/a06c8150-e5ff-44db-9bf8-a385fdb039b8

### 程式
```C
const int Red = 9;
const int Green = 10;
const int Blue = 11;

void setup()
{
  pinMode(Red, OUTPUT);
  pinMode(Green, OUTPUT);
  pinMode(Blue, OUTPUT);
}

void colorTransition(int pin1, int pin2, int pin3) {
  for (int i = 0; i <= 255; i++) {
    analogWrite(pin1, i);
    analogWrite(pin2, 0);
    analogWrite(pin3, 0);
    delay(10);
  } // from dark to bright

  for (int i = 255; i >= 0; i--) {
    analogWrite(pin1, i);
    analogWrite(pin2, 0);
    analogWrite(pin3, 0);
    delay(10);
  } // from bright to dark
  delay(1000);
}

void loop() {
  // Red
  colorTransition(Red, Green, Blue);
  
  // Green
  colorTransition(Green, Red, Blue);
  
  // Blue
  colorTransition(Blue, Red, Green);
}
```
