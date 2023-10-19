# ES2023 - 實作4: 七段顯示器, LCD 顯示器 + 超音波感測器 (2W)

## Lab 4-2 如下圖的Demo, 用七段顯示器, 顯示 . →1→ ... → 9 → 0 → 全滅, 狀態改變的間隔時間為0.5秒

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/a8c33c22-7223-4371-9538-b1820c8217e5

### 程式
```C
#include <Adafruit_NeoPixel.h>

// 設定 neopixel ring 的引腳和數量
#define NEOPIXEL_RING_24_PIN A0
#define NEOPIXEL_RING_24_NUM 24
#define NEOPIXEL_RING_16_PIN A1
#define NEOPIXEL_RING_16_NUM 16
#define NEOPIXEL_JEWEL_PIN A2
#define NEOPIXEL_JEWEL_NUM 8

// 設定 neopixel ring 的物件
Adafruit_NeoPixel neopixel_ring_24 = Adafruit_NeoPixel(NEOPIXEL_RING_24_NUM, NEOPIXEL_RING_24_PIN, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel neopixel_ring_16 = Adafruit_NeoPixel(NEOPIXEL_RING_16_NUM, NEOPIXEL_RING_16_PIN, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel neopixel_jewel = Adafruit_NeoPixel(NEOPIXEL_JEWEL_NUM, NEOPIXEL_JEWEL_PIN, NEO_GRB + NEO_KHZ800);

// 設定動畫的顏色
const uint32_t color_1 = Adafruit_NeoPixel::Color(255, 0, 0);
const uint32_t color_2 = Adafruit_NeoPixel::Color(0, 255, 0);
const uint32_t color_3 = Adafruit_NeoPixel::Color(0, 0, 255);

// 動畫函式
void animation() {
  // 將 neopixel ring 24 填滿顏色 1
  for (int i = 0; i < NEOPIXEL_RING_24_NUM; i++) {
    neopixel_ring_24.setPixelColor(i, color_1);
  }
  neopixel_ring_24.show();
  delay(500);
  neopixel_ring_24.fill(Adafruit_NeoPixel::Color(0, 0, 0));
  neopixel_ring_24.show();

  // 將 neopixel ring 16 填滿顏色 2
  for (int i = 0; i < NEOPIXEL_RING_16_NUM; i++) {
    neopixel_ring_16.setPixelColor(i, color_2);
  }
  neopixel_ring_16.show();
  delay(500);
  neopixel_ring_16.fill(Adafruit_NeoPixel::Color(0, 0, 0));
  neopixel_ring_16.show();

  // 將 neopixel jewel 填滿顏色 3
  for (int i = 0; i < NEOPIXEL_JEWEL_NUM; i++) {
    neopixel_jewel.setPixelColor(i, color_3);
  }
  neopixel_jewel.show();
  delay(500);
  neopixel_jewel.fill(Adafruit_NeoPixel::Color(0, 0, 0));
  neopixel_jewel.show();
}

int numbers[10][8] = {
  {1, 1, 1, 1, 1, 1, 0, 1}, // 0
  {0, 1, 1, 0, 0, 0, 0, 1}, // 1
  {1, 1, 0, 1, 1, 0, 1, 1}, // 2
  {1, 1, 1, 1, 0, 0, 1, 1}, // 3
  {0, 1, 1, 0, 0, 1, 1, 1}, // 4
  {1, 0, 1, 1, 0, 1, 1, 1}, // 5
  {1, 0, 1, 1, 1, 1, 1, 1}, // 6
  {1, 1, 1, 0, 0, 0, 0, 1}, // 7
  {1, 1, 1, 1, 1, 1, 1, 1}, // 8
  {1, 1, 1, 1, 0, 1, 1, 1}  // 9
};

int redPin = 9;    // 紅色LED的腳位
int greenPin = 10;  // 綠色LED的腳位
int bluePin = 11;   // 藍色LED的腳位
int interval = 500; // 閃爍間隔，以毫秒為單位

void seg71(int* number)
{
  for(int i = 0; i < 8; i++) {
    digitalWrite(i + 1, number[i]);
  }
  delay(1000);
}
void seg()
{
  for(int i = 1; i <= 9; i++) {
    seg71(numbers[i]);
	animation();
	LED();
  }
  seg71(numbers[0]); // 0
}

void LED()
{
  analogWrite(redPin, 255);
  delay(interval);
  analogWrite(redPin, 0);
  analogWrite(greenPin, 255);
  delay(interval);
  analogWrite(greenPin, 0);
  analogWrite(bluePin, 255);
  delay(interval);
  analogWrite(bluePin, 0);
}


// 設定LED的顏色
void setColor(int red, int green, int blue) {
  analogWrite(redPin, red);
  analogWrite(greenPin, green);
  analogWrite(bluePin, blue);
}

void setup()
{
  for(int x = 1; x < 9; x++) {
    pinMode(x, OUTPUT);
  }
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
    // 初始化 neopixel ring
  pinMode(A0, OUTPUT);
  pinMode(A1, OUTPUT);
  pinMode(A2, OUTPUT);
  neopixel_ring_24.begin();
  neopixel_ring_16.begin();
  neopixel_jewel.begin();
}

void loop()
{
  seg();
}
```
