# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-4 analogRead(), 1024解析度 (i.e.,10-bit): 可變電阻 + 序列監視器與輸出; 當你改變可變電阻的阻值(e.g., 10K-ohm)時，序列監視器輸出的數值有什麼改變? 數值又有什麼意義呢? 可試將你的想法寫在你的GitHub Page中喔! (互動4) (2W)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/7c867a6b-b6e1-4fcc-93cb-b3195fd6ca46

### 程式
```C
int sensorValue = 0;

void setup()
{
  pinMode(A0, INPUT);
  Serial.begin(9600);
}

void loop()
{
  // read the input on analog pin 0:
  sensorValue = analogRead(A0);
  // print out the value you read:
  Serial.println(sensorValue);
  delay(10); // Delay a little bit to improve simulation performance
}
```
