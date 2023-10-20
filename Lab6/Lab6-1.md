# ES2023 - 實作6: 4X4鍵盤 + LCD 顯示器 + 回顧 + AI-based嵌入式系統? (1W)

## Lab 6-1 用16X2 LCD 顯示器來顯示4X4鍵盤輸入的數字 (0, 1, 2, .., 9), 若輸入的字數≥16則換到下一列, 若兩皆滿, 則清除劃面重新由Row=0, Col=0開始

### 電路


### 程式
```C
#include <LiquidCrystal.h>
#include <Keypad.h>

LiquidCrystal lcd_1(12, 11, 5, 4, 3, 2);  // 定義LCD物件，輸入引腳為12, 11, 5, 4, 3, 2

const byte ROWS = 4;  // 鍵盤的列數(橫的)
const byte COLS = 4;  // 鍵盤的行數(直的)
char keys[ROWS][COLS] = {  // 鍵盤的按鍵配置
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {A0, A1, 13, 10};  // 定義鍵盤的列的腳位
byte colPins[COLS] = {9, 8, 7, 6};  // 定義鍵盤的行的腳位

int LCDCol = 0;  // LCD的列位置
int LCDRow = 0;  // LCD的行位置

Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );  // 定義鍵盤物件

void setup() {
   Serial.begin(9600);  // 初始化串口通信，波特率9600
   lcd_1.begin(16, 2);  // 初始化LCD，16列，2行
   lcd_1.setCursor(LCDCol, LCDRow);  // 設定LCD的初始位置
}

void loop() {
  char key = keypad.getKey();  // 讀取鍵盤的輸入

  if (key) {  // 如果有按鍵輸入
    Serial.println(key);  // 將按鍵值輸出到串口
  
    if (LCDCol > 15) {  // 如果LCD的列位置超出範圍
      ++LCDRow;  // 將LCD的行位置增加
    
      if (LCDRow > 1) {  // 如果LCD的行位置超出範圍
        LCDRow = 0;  // 重置LCD的行位置
        LCDCol = 0;  // 重置LCD的列位置
        lcd_1.clear();  // 清除LCD上的內容
      }
    
      LCDCol = 0;  // 重置LCD的列位置
    }
  
    lcd_1.setCursor(LCDCol, LCDRow);  // 設定LCD的位置
    lcd_1.print(key);  // 在LCD上顯示按鍵值
    ++LCDCol;  // 將LCD的列位置增加
  }
}
```
