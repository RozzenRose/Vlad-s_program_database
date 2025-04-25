#redis 

Модуль для работы с **Redis**
```power shell
pip install redis
```
Подключение и проверка:
```python
import redis  
  
r = redis.Redis(host='192.168.50.196', port=6379)  
r.set('foo', 'bar')  
print(r.get('foo'))
```