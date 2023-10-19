# ES2023 - 實作4: 七段顯示器, LCD 顯示器 + 超音波感測器 (2W)

## ## Lab 4-4 整合超音波感測器 + LCD + LED: 參考之前的實作, 完成以下任務:

### 1. **將超音波感測器傳回的距離, 在LCD上面顯示,**

### 2. **同時也和之前的實作一樣, 在序列輸出.**

### 3. **另外, 當物體的距離小於150cm時, 則亮紅色LED, 否則亮綠色LED**

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/e03b7f82-f64f-4b6d-ad4b-424fe7807a1b
### 程式
```C
// C++ code
#include <Adafruit_NeoPixel.h>
// include the library code:
#include <LiquidCrystal.h>

// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

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
  // set up the LCD's number of columns and rows:
  lcd.begin(16, 2);
  // Print a message to the LCD.
  lcd.print("Distance, cm:");
}

void loop()
{
  // measure the ping time in cm
  int cm = 0.01723 * readUltrasonicDistance(7, 7);
  
  // 根據距離改變LED顏色，形成漸層變化
  int red, green, blue;
  if (cm < 70) {
    red = 255;
    green = 0;
    blue = 0;
  } else if (cm > 270) {
    red = 0;
    green = 0;
    blue = 255;
  } else {
    // 計算漸層變化的顏色值
    int range = 270 - 70;
    int step = 255 / range;
    int intensity = (cm - 70) * step;
    red = 255 - intensity;
    green = intensity;
    blue = 0;
  }

  setColor(red, green, blue);
  Serial.print(cm);
  Serial.println("cm");
  lcd.setCursor(0, 1);
  // print the number of seconds since reset:
  lcd.print(cm);
  delay(100); // 等待100毫秒
}

void setColor(int red, int green, int blue) {
  for (int i = 0; i < NUMPIXELS; i++) {
    pixels.setPixelColor(i, pixels.Color(red, green, blue));
  }
  pixels.show(); // 顯示設定的顏色
}
```
