# ES2023 - 實作4: 七段顯示器, LCD 顯示器 + 超音波感測器 (2W)

## Lab 4-1 用七段顯示器來顯示數字"8."

### 電路

https://github.com/knnv5h/ES-Fall2023/assets/43922704/59277d20-7cb5-4be0-9d6f-cf6c9ddffb39

### 程式
```C
void setup()
{
	for(int x = 1; x < 9; x++) {
		pinMode(x,OUTPUT);
	}
}

void seg71(int a, int b, int c, int d, int e, int f, int g, int h)
{
digitalWrite(1, a);
digitalWrite(2, b);
digitalWrite(3, c);
digitalWrite(4, d);
digitalWrite(5, e);
digitalWrite(6, f);
digitalWrite(7, g);
digitalWrite(8, h);
delay(500);
}

void loop()
{
//    a, b, c, d, e, f, g, h
seg71(0, 0, 0, 0, 0, 0, 0, 0); // OFF
seg71(1, 1, 1, 1, 1, 1, 1, 1); // 8
}
```
