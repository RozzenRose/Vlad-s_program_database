#python #функция 

![[Pasted image 20240605111519.png]]
Еще называют анонимными функциями

Иногда бывают ситуации, когда функции нам нужна единственный раз, для таких функций удобнее использовать синтаксис lambda-функций. Они всегда ограниченны всего одним выражением и не могут содержать инструкции `return`
Объявление:
```python
lambda список_параметров: выражение
```
```python
lambda_function = lambda x: x+2 #объявление lambda функции
```
Пример:
```python
f1 = lambda: 10 + 20 # функция без параметров
f2 = lambda х, у: х + у # функция с двумя параметрами
f3 = lambda х, у, z: х + у + z # функция с тремя параметрами

print(f1())
print(f2(5, 10))
print(f3(5, 10, 30))
```
```
30
15
45
```

Можно использовать условный оператор через конструкцию
```python
значение1 if условие else значение2

numbers = [-2, 0, 1, 2, 17, 4, 5, 6]
result = list(map(lambda x: 'even' if x % 2 == 0 else 'odd', numbers))
print(result)
```

```
['even', 'even', 'odd', 'even', 'odd', 'even', 'odd', 'even']
```

Примеры с передачей аргументов:
```python
f1 = lambda x, y, z: x + y + z
f2 = lambda x, y, z=3: x + y + z
f3 = lambda *args: sum(args)
f4 = lambda **kwargs: sum(kwargs.values())
f5 = lambda x, *, y=0, z=0: x + y + z

print(f1(1, 2, 3))
print(f2(1, 2))
print(f2(1, y=2))
print(f3(1, 2, 3, 4, 5))
print(f4(one=1, two=2, three=3))
print(f5(1))
print(f5(1, y=2, z=3))
```

Можно вызвать сразу после определения:
```python
print((lambda х, у: х + у)(5, 10)) # 5 + 10
print(1 + (lambda x: x*5)(10) + 2) # 1 + 50 + 2
```

Через `lambda` функции можно реализовать рекурсию, если присвоить ее к переменной и в теле самой функции обращаться к этой переменной:
```python
fact = lambda n: 1 if n == 0 else n*fact(n - 1)

for i in range(10):
    print(fact(i))
```
```
1
1
2
6
24
120
720
5040
40320
362880
```