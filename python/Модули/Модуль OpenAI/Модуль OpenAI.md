#python #LLM #API #DeepSeek #chatGPT 

**OpenAI** - это библиотека для работы с внешними API всяких текстовых нейросетей(**DeepSeek**, **chatGPT**)
Синк:
```python
pip install openai
```
Библиотека **openai** построена на строгой объектно-ориентированной архитектуре. Ее ядро - это центральный клиентский объект, который предоставляет доступ к различным API-интерфейсам, а те, в свою очередь, возвращают данные в виде четко структурированных моделей.
![[deepseek_mermaid_20251217_14ab1a 1.png]]

### Клиент (The Client)
Это точка входа. Объект, через который происходит все взаимодействие с внешним API. Инициализируется с передачей ключа от API.
```python
from openai import OpenAI
client = OpenAI(api_key="ваш_ключ")  # Синхронный клиент

from openai import AsyncOpenAI
async_client = AsyncOpenAI(api_key="ваш_ключ")  # Асинхронный клиент
```
### Ресурсы (Resources)
Через клиент вы получаете доступ к ресурсам - это основные разделы API. Они доступны как атрибуты объекта клиента:
```python
# Основные ресурсы
client.chat          # Для работы с чат-моделями (GPT)
client.completions   # Для устаревших text-моделей (Davinci и др.)
client.images        # Для генерации изображений (DALL-E)
client.embeddings    # Для создания эмбеддингов
client.audio         # Для работы с аудио (Whisper, TTS)
client.models        # Для получения списка доступных моделей
client.files         # Для загрузки и управления файлами
```
### Методы ресурсов
У каждого ресурса есть методы (например `create()`) для выполнения операций. Вложенность - ключевая особенность библиотеки
```python
# Классический путь для создания чат-запроса
response = client.chat.completions.create(...)
		#    │      │    │          └── Метод ресурса
		#    │      │    └────── Конкретный ресурс (completions внутри chat)
		#    │      └────────────────── Ресурс верхнего уровня (chat)
		#    └─────────────────────────── Объект клиента
```
### Объекты ответа
Метод `.create()` возвращает модель Pydantyc - объект с четкими полями, а не просто словарь. Это упрощает доступ к данным.
```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Привет!"}]
)

# response — это объект класса ChatCompletion
print(type(response))  # <class 'openai.types.chat.chat_completion.ChatCompletion'>

# Доступ к полям через точку (не как к словарю)
print(response.id)  # Уникальный ID запроса
print(response.created)  # Временная метка
print(response.model)  # Использованная модель

# Самый важный атрибут — choices (список объектов Choice)
first_choice = response.choices[0]
print(type(first_choice))  # <class 'openai.types.chat.chat_completion.Choice'>

# Внутри Choice лежит объект ChatCompletionMessage
message = first_choice.message
print(type(message))  # <class 'openai.types.chat.chat_completion.ChatCompletionMessage'>
print(message.role)   # 'assistant'
print(message.content) # Текст ответа модели
```

### Пример
```python
import os
from openai import OpenAI

# Инициализация клиента для DeepSeek
client = OpenAI(
    api_key=os.environ.get("DEEPSEEK_API_KEY"),
    base_url="https://api.deepseek.com"  # Указываем endpoint DeepSeek
)

# Отправка запроса
response = client.chat.completions.create(
    model="deepseek-chat",  # Основная модель для чата
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Привет! Кто ты?"}
    ],
    stream=False  # Установите True для потокового ответа
)

print(response.choices[0].message.content)
```
В объект `message` передается два словаря, один из которых содержит значение `system` по ключу `role`, а другой `user`. В первом случае по ключу `content` мы помещаем контекст, которые рассказывает LLM кто он такой, можно так же указать какие то правила для ответов. Во втором случае мы размещаем промт.