# ES2023 - 實作5: Servo伺服馬達, 溫度感應器 + LCD 顯示器,  (1W)

## Lab 5-2 LCD顯示溫度感應器的溫度;若溫度<38 綠LED亮; 若大於38度, 紅色LED亮

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/a1b94180-1638-4775-bd9d-40f2d370133f

### 程式
```C
#include <LiquidCrystal.h>
#include <Adafruit_NeoPixel.h>

#define PIN 6 // NeoPixel Ring 12的資料腳位連接到Arduino的第6腳位
#define NUMPIXELS 12 // NeoPixel Ring 12有12個LED

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800); // 初始化NeoPixel物件
LiquidCrystal lcd(12, 11, 5, 4, 3, 2); // LCD物件，設定腳位

void setup()
{
  lcd.begin(16, 2); // 設定LCD顯示列數和行數
  Serial.begin(9600); // 初始化串列通訊
  pixels.begin(); // 初始化NeoPixel Ring  
  pinMode(A0, INPUT); // 設定A0為輸入模式
}

void loop()
{
  int reading = analogRead(A0);  // 讀取類比訊號 (2^10的數值)
  lcd.setCursor(0,0);  
  lcd.print("TMP Sensor Demo"); // 在LCD上顯示標題
  float voltage = (reading/1024.0) * 5.0; // 將讀值轉換為電壓  
  float tempC = (voltage - 0.5) * 100.0; // 將電壓轉換為攝氏溫度
  lcd.setCursor(0,1);
  lcd.print("Tmp:");
  lcd.print(tempC);
  lcd.print(" C");
  
  // 根據溫度值計算RGB顏色
  int color = map(tempC, -40, 125, 0, 255);
  uint32_t finalColor = pixels.Color(color, 255 - color, 0); // 由藍轉紅
  
  // 將NeoPixels的每顆LED設定為計算後的顏色
  for(int i=0; i<NUMPIXELS; i++) {
    pixels.setPixelColor(i, finalColor);
  }
  
  pixels.show(); // 更新NeoPixels顯示
  delay(500); // 延遲0.5秒
  lcd.clear(); // 清除LCD顯示
  Serial.println(reading); // 將讀值印出至串列監視器
  Serial.println(voltage); // 將電壓值印出至串列監視器
}
```
