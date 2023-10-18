# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-5: 按下按鍵, Green LED亮 & Red LED滅; 放開按鍵, Green LED滅 & Red LED亮. 想要再深入的同學可以試試喔. (思考方向: digitalRead(), digitalWrite(): 按鍵 +序列輸出 + LED), (互動5) (2W)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/3a6cf80e-00cf-48db-ad17-001faee474de

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
