#calendar #Python #метод #month


Вернет календарь на месяц в многострочной строке. при объявлении указывается год, месяц, ширина и количество строк

`month(year, month, w=0, l=0)` 
`year` - год
`month` - месяц
`w` - ширина столбца даты (необязательный)
`h` - количество строк, отводимые на неделю (необязательный)
```python
import calendar

print(calendar.month(2021, 9))
print(calendar.month(2021, 10))
print(calendar.month(2021, 9, w=3))
print(calendar.month(2021, 9, l=2))
print(calendar.month(2021, 9, w=5, l=2))
```
```
   September 2021
Mo Tu We Th Fr Sa Su
       1  2  3  4  5
 6  7  8  9 10 11 12
13 14 15 16 17 18 19
20 21 22 23 24 25 26
27 28 29 30

    October 2021
Mo Tu We Th Fr Sa Su
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31

       September 2021
Mon Tue Wed Thu Fri Sat Sun
          1   2   3   4   5
  6   7   8   9  10  11  12
 13  14  15  16  17  18  19
 20  21  22  23  24  25  26
 27  28  29  30

   September 2021

Mo Tu We Th Fr Sa Su

       1  2  3  4  5

 6  7  8  9 10 11 12

13 14 15 16 17 18 19

20 21 22 23 24 25 26

27 28 29 30


              September 2021

 Mon   Tue   Wed   Thu   Fri   Sat   Sun

               1     2     3     4     5

   6     7     8     9    10    11    12

  13    14    15    16    17    18    19

  20    21    22    23    24    25    26

  27    28    29    30
```