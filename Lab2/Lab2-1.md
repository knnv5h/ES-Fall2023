# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-1, analogWrite(): 並且觀查LED亮度變化是否有像"呼吸的效果"和示波器的波形有什麼關連性? (互動1), (1W: ~ 01:45pm)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/94275787-fc9f-499d-96cf-be79cf7ef727

### 程式
```C
int brightness = 0;

void setup()
{
  pinMode(9, OUTPUT);
}

void loop()
{
  for (brightness = 0; brightness <= 255; brightness += 5) {
    analogWrite(9, brightness);
    delay(30);
  }
  for (brightness = 255; brightness >= 0; brightness -= 5) {
    analogWrite(9, brightness);
    delay(30);
  }
}
```
