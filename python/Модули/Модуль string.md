#модуль #python #string

```python
import string
from string import*
```
В модуле `string` Python есть много полезных констант и классов для работы со строками. Вот некоторые из них:
- **Константы**:
	- `string.ascii_letters` - конкатенация `ascii_lowercase` и `ascii_uppercase`.
	- `string.ascii_lowercase` - строчные буквы английского алфавита.
    - `string.ascii_uppercase` - заглавные буквы английского алфавита.
    - `string.digits` - цифры от 0 до 9.
    - `string.hexdigits` - шестнадцатеричные цифры.
    - `string.octdigits` - восьмеричные цифры.
    - `string.punctuation` - пунктуационные символы.
    - `string.printable` - печатаемые символы.
    - `string.whitespace` - символы пробела.
- **Функции**:
    - `string.capwords(s, sep=None)` - преобразует первую букву каждого слова в верхний регистр.
- **Классы**:
    - `string.Formatter` - позволяет создавать и настраивать собственные поведения форматирования строк.
    - `string.Template` - используется для создания шаблонов строк для более простых подстановок строк