# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-5: 按下按鍵, Green LED亮 & Red LED滅; 放開按鍵, Green LED滅 & Red LED亮. 想要再深入的同學可以試試喔. (思考方向: digitalRead(), digitalWrite(): 按鍵 +序列輸出 + LED), (互動5) (2W)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/3a6cf80e-00cf-48db-ad17-001faee474de

### 程式
```C
#include <Adafruit_NeoPixel.h>

const int buttonPin = 2;
const int ledPins[] = {7, 8, 13};
const int neoPixelPin = 3;  // NeoPixel Jewel的接腳
const int numberOfPixels = 7;  // NeoPixel Jewel上LED的數量

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(numberOfPixels, neoPixelPin, NEO_GRB + NEO_KHZ800);

int buttonState = 0;
int lastButtonState = 0;
int currentLed = 0;

void setup() {
  pinMode(buttonPin, INPUT);
  for (int i = 0; i < 3; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
  pixels.begin();  // 初始化NeoPixel Jewel
}

void loop() {
	cycleLedsWithButton();
	lastButtonState = buttonState;
}

void cycleLedsWithButton(){
	buttonState = digitalRead(buttonPin);
    if (buttonState == HIGH && lastButtonState == LOW) {
      digitalWrite(ledPins[currentLed], LOW);
      pixels.clear();  // 關閉所有NeoPixel LED
      pixels.show();
      
      currentLed = (currentLed + 1) % 3;

      digitalWrite(ledPins[currentLed], HIGH);
      if (currentLed == 0) {
        // 第7腳亮時，NeoPixel Jewel用藍色快速繞圓圈動畫
        for (int i = 1; i < 500; i++){
          colorWipe(0x0000FF, 100);
        }
      } else if (currentLed == 1) {
        // 第8腳亮時，NeoPixel Jewel用紅色快速繞圓圈動畫
        for (int i = 1; i < 500; i++){
          colorWipe(0xFF0000, 100);
        }
      } else if (currentLed == 2) {
        // 第13腳亮時，NeoPixel Jewel用綠色快速繞圓圈動畫
        for (int i = 1; i < 500; i++){
          colorWipe(0x00FF00, 100);
        }  
      }
    }
}

void colorWipe(uint32_t color, int wait) {
  for (int i = 1; i < pixels.numPixels(); i++) {
    pixels.setPixelColor(i, color);
    pixels.show();
    delay(wait);
    pixels.setPixelColor(i, 0);
	cycleLedsWithButton();
    delay(100);  // 延遲0.1秒，讓繞圈動畫顯示一段時間
  }
}
```
