# ES2023 - 實作4: 七段顯示器, LCD 顯示器 + 超音波感測器 (2W)

## Lab 4-3 LCD顯示"Hello" + 你的英文名字 (e.g., "Hello Horace")

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/cda4f92c-1698-4744-a559-3496016af827

### 程式
```C
// include the library code:
#include <LiquidCrystal.h>

// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
  // set up the LCD's number of columns and rows:
  lcd.begin(16, 2);
  // Print a message to the LCD.
  lcd.print("hello, Leo!");
}

void loop() {
  // set the cursor to column 0, line 1
  // (note: line 1 is the second row, since counting begins with 0):
  lcd.setCursor(0, 1);
  // print the number of seconds since reset:
  lcd.print(millis() / 100);
}
```
