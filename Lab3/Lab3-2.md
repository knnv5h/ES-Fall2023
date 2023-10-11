# ES2023 - 實作3: 使用超音波感測器 + LED控制, C語言程式速成 (1W)

## Lab 3-2: 超音波感測器 + LED (紅色LED:亮<70cm, 藍色LED: 亮>270cm, 緑色LED:亮, 介於70cm ~ 270cm之間) + RS232 Output

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/1cfd6de5-f485-4a24-bd07-daf23a4c3c69
### 程式
```C
// C++ code
#include <Adafruit_NeoPixel.h>

#define PIN 6 // NeoPixel Ring 12的資料腳位連接到Arduino的第6腳位
#define NUMPIXELS 12 // NeoPixel Ring 12有12個LED

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

long readUltrasonicDistance(int triggerPin, int echoPin)
{
  pinMode(triggerPin, OUTPUT);  // Clear the trigger
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  // Sets the trigger pin to HIGH state for 10 microseconds
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  // Reads the echo pin, and returns the sound wave travel time in microseconds
  return pulseIn(echoPin, HIGH);
}

void setup()
{
  Serial.begin(9600);
  pixels.begin(); // 初始化NeoPixel Ring
}

void loop()
{
  // measure the ping time in cm
  int cm = 0.01723 * readUltrasonicDistance(7, 7);
  
  // 根據距離改變LED顏色
  if (cm < 70) {
    setColor(255, 0, 0); // 紅色
  } else if (cm > 270) {
    setColor(0, 0, 255); // 藍色
  } else {
    setColor(0, 255, 0); // 綠色
  }

  Serial.print(cm);
  Serial.println("cm");
  delay(100); // Wait for 100 millisecond(s)
}

void setColor(int red, int green, int blue) {
  for (int i = 0; i < NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(red, green, blue));
  }
  pixels.show(); // 顯示設定的顏色
}
```
