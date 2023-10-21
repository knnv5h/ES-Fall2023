# ES2023 - 實作7: AI-based ES? AI? ML? DL? 要如何入門 (How To Learn)? (1W)

## Lab 7-2 十分鐘學會Python!? Really?
![螢幕擷取畫面 2023-10-21 194847](https://github.com/knnv5h/ES-Fall2023/assets/43922704/681dfaf9-44da-4fd7-b393-04ddc51dbcca)

### 程式
``` python
# task 1: string variable
name = "Leo"
print(name)

# task 2: number variable
number = 30
print(number)
print(number*3)
print(number/2)
print(number + number)
print(number - number)

# task 3: function

def printName(firstName, lastName):
  print(lastName + ' ' + firstName)

printName('Leo', 'Zou')

# task 4: if else

def printName(firstName, lastName, isCool):
  if isCool:
    print(lastName + ' ' + firstName + ' very cool!')
  else:
    print(lastName + ' ' + firstName + ' not cool!')

# Start
printName('Leo', 'Zou', True)
printName('Leo', 'Zou', False)

# task 5: for loop

def printName(firstName, lastName, isCool, num):
  for i in range(1, num):
    if isCool:
      print(i, lastName + ' ' + firstName + ' very cool!')
    else:
      print(i, lastName + ' ' + firstName + ' not cool!')

# Start
printName('Leo', 'Zou', True, 10)
```
