# ES2022 - 實作1: Welcome: GitHub入門, Tinker CAD, digitalWrite(), LED亮滅

## 1-3 在TinkerCAD開一個新的Circuit, 分別使甪R, G, B三種顏色的LED, 讓LED ON, OFF的順序為R → G → B → G → R .... 無限循環

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/44c30745-dcde-481a-9319-5337b4859a90

### 程式
```C

void setup()
{
  pinMode(13, OUTPUT);
  pinMode(11, OUTPUT);
  pinMode(9, OUTPUT);
}

void loop()
{
  digitalWrite(13,HIGH);
  digitalWrite(11,LOW);
  digitalWrite(9, LOW);
  delay(500);
  digitalWrite(13,LOW );
  digitalWrite(11, HIGH);
  digitalWrite(9, LOW);
  delay(500);
  digitalWrite(13,LOW );
  digitalWrite(11, LOW);
  digitalWrite(9, HIGH);
  delay(500);
  digitalWrite(13, LOW);
  digitalWrite(11, HIGH);
  digitalWrite(9, LOW);
  delay(500);
}
```
