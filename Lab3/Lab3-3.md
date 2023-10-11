# ES2023 - 實作3: 使用超音波感測器 + LED控制, C語言程式速成 (1W)

## Lab 3-3: Arudino常用的C語言程式介紹與實作

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/6c89ef8f-3df7-42f8-8439-fbebe6f866ad

### 程式

```C
#include <Adafruit_NeoPixel.h>

#define PIN 6
#define NUMPIXELS 12

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  Serial.begin(9600);
  pixels.begin();
}

void loop() {
  // 初始化 NeoPixel 環 12 的 LED 狀態，前六個 LED 紅色，後六個 LED 綠色（最暗）
  for (int i = 0; i < NUMPIXELS; i++) {
    if (i < 6) {
      pixels.setPixelColor(i, pixels.Color(255, 0, 0));  // 紅色
    } else {
      pixels.setPixelColor(i, pixels.Color(0, 0, 0));  // 綠色初始狀態為關閉
    }
  }
  pixels.show();

  // 印出1x1到9x9的乘法表
  for (int i = 1; i <= 9; i++) {
    int aa = 0;

    for (int j = 1; j <= 9; j++) {
      int result = i * j;
      String d1 = String(i) + "x" + String(j) + "=" + String(result)+" ";
      Serial.print(d1);

      for (int k = 6; k < NUMPIXELS; k++) {
        int brightness = map(aa, 0, 9, 0, 255);  // 映射亮度從0到255
        pixels.setPixelColor(k, pixels.Color(0, brightness, 0));  // 綠色，逐漸變亮
        pixels.show();
        delay(50);  // 調整亮度變化速度
        aa++;
      }
      delay(100);
    }  // loop j
    Serial.println(" ");
	Serial.println(" ");
  }  // loop i

  // 關閉紅色 LED
  for (int i = 0; i < 6; i++) {
    pixels.setPixelColor(i, pixels.Color(0, 0, 0));  // 紅色關閉
  }
  pixels.show();

  // 等待片刻
  delay(5000);
}
```
