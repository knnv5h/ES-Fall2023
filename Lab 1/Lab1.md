# ES2022 - 實作1: Welcome: GitHub入門, Tinker CAD, digitalWrite(), LED亮滅

## 1-1 在TinkerCAD開一個新的Circuit, 加上一個外部的LED, 並且ON (亮) 0.5秒, OFF(滅) 0.5秒

![image](https://github.com/knnv5h/ES-Fall2023/assets/43922704/4002a9cb-5814-4a74-974d-2365d7407a9e)

### 程式

```
// C++ code
//
/*
  This program blinks pin 13 of the Arduino (the
  built-in LED)
*/

void setup()
{
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop()
{
  // turn the LED on (HIGH is the voltage level)
  digitalWrite(LED_BUILTIN, HIGH);
  delay(500); // Wait for 1000 millisecond(s)
  // turn the LED off by making the voltage LOW
  digitalWrite(LED_BUILTIN, LOW);
  delay(500); // Wait for 1000 millisecond(s)
}
```C++

## 1-2 在TinkerCAD開一個新的Circuit, 分別使甪R, G, B三種顏色的LED, ALL ON (亮) 0.5秒, OFF(滅) 0.5秒

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/5450184b-4fdb-4c9d-b740-2368d442f371

### 程式
```
// C++ code
//
/*
  This program blinks pin 13 of the Arduino (the
  built-in LED)
*/

void setup()
{
  pinMode(13, OUTPUT);
  pinMode(11, OUTPUT);
  pinMode(9, OUTPUT);
}

void loop()
{
  // turn the LED on (HIGH is the voltage level)
  digitalWrite(13, HIGH);
  digitalWrite(11, HIGH);
  digitalWrite(9, HIGH);
  delay(500); // Wait for 1000 millisecond(s)
  // turn the LED off by making the voltage LOW
  digitalWrite(13, LOW);
  digitalWrite(11, LOW);
  digitalWrite(9, LOW);
  delay(500); // Wait for 1000 millisecond(s)
}
``` 
