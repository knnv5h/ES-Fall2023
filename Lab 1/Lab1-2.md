## 1-2 在TinkerCAD開一個新的Circuit, 分別使甪R, G, B三種顏色的LED, ALL ON (亮) 0.5秒, OFF(滅) 0.5秒

### 電路
https://github.com/knnv5h/ES-Fall2023/assets/43922704/5450184b-4fdb-4c9d-b740-2368d442f371

### 程式
```C
// C++ code

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
```****
