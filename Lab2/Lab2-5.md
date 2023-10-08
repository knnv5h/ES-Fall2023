# 實作2: 會呼吸的RGB LED,  按鍵控制, 狀態輸出

## 實作2-5: 按下按鍵, Green LED亮 & Red LED滅; 放開按鍵, Green LED滅 & Red LED亮. 想要再深入的同學可以試試喔. (思考方向: digitalRead(), digitalWrite(): 按鍵 +序列輸出 + LED), (互動5) (2W)

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/34f27d4b-fb7c-498d-ba56-7b1d6586f9e1

### 程式
```C
int buttonState = 0;
int GLED = 13;
int RLED = 8;
void setup()
{
  pinMode(2, INPUT);
  
  pinMode(GLED, OUTPUT);
  pinMode(RLED, OUTPUT);
  
  Serial.begin(9600);
}

void loop()
{
  // read the state of the pushbutton value
  buttonState = digitalRead(2);
  // check if pushbutton is pressed.  if it is, the
  // buttonState is HIGH
  if (buttonState == HIGH) {
    // turn LED on
    digitalWrite(GLED, HIGH);
    digitalWrite(RLED, LOW);
    
  } else {
    // turn LED off
    digitalWrite(GLED, LOW);
    digitalWrite(RLED, HIGH);
  }
  Serial.println(buttonState);
  delay(10); // Delay a little bit to improve simulation performance
}
```
