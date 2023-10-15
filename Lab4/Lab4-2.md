# ES2023 - 實作4: 七段顯示器, LCD 顯示器 + 超音波感測器 (2W)

## Lab 4-2 如下圖的Demo, 用七段顯示器, 顯示 . →1→ ... → 9 → 0 → 全滅, 狀態改變的間隔時間為0.5秒

### 電路


### 程式
```C
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
int delayTime = 10; // 延遲時間，控制顏色變換的速度

void seg71(int* number)
{
  for(int i = 0; i < 8; i++) {
    digitalWrite(i + 1, number[i]);
  }
  delay(1000);
}

void rainbowCycle() {
  byte i;
  for(i = 0; i < 255; i++) {
    setColor(255 - i, i, 0); // 紅->黃
    delay(delayTime);
  }
  for(i = 0; i < 255; i++) {
    setColor(0, 255 - i, i); // 黃->綠
    delay(delayTime);
  }
  for(i = 0; i < 255; i++) {
    setColor(i, 0, 255 - i); // 綠->藍
    delay(delayTime);
  }
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
}

void loop()
{
  for(int i = 1; i <= 9; i++) {
    seg71(numbers[i]);
  }
  seg71(numbers[0]); // 0
  rainbowCycle();
}
```
