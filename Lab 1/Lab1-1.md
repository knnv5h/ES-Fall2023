# ES2022 - 實作1: Welcome: GitHub入門, Tinker CAD, digitalWrite(), LED亮滅

 ## 1-1 在TinkerCAD開一個新的Circuit, 加上一個外部的LED, 並且ON (亮) 0.5秒, OFF(滅) 0.5秒

 ### 電路
![image](https://github.com/knnv5h/ES-Fall2023/assets/43922704/4002a9cb-5814-4a74-974d-2365d7407a9e)

### 程式
```C
// C++ code

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
```
