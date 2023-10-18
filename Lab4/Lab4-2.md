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
}

void loop()
{
  LED();
  seg();
}
```
