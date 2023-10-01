# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-2, RGB LED燈全彩模組, 分別讓LED輪流表現正紅、正綠、正藍，三個顏色，時間間隔1秒鐘。並且觀查LED顏色和波形或是電壓有什麼關連性? 可將個人說明在更新GitHub時一起加入. (互動2), (1W: ~02:20pm)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/ac7eb0cc-f29f-4a22-9f36-8c69c845accc

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

void loop()
{
  // turn the LED on (HIGH is the voltage level)
  analogWrite(Red, 255);
  analogWrite(Green, 0);
  analogWrite(Blue, 0);
  delay(1000); // Wait for 1000 millisecond(s)
  
  analogWrite(Red, 0);
  analogWrite(Green, 255);
  analogWrite(Blue, 0);
  delay(1000); // Wait for 1000 millisecond(s)
  
  analogWrite(Red, 0);
  analogWrite(Green, 0);
  analogWrite(Blue, 255);
  delay(1000); // Wait for 1000 millisecond(s)  
}
```
