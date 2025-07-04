#Linux

Если дать этой команде PID, она расскажет, какому процессу он принадлежит:
```bash
ps -fp 1234
```
 
 Пример вывода:
```bash
UID        PID  PPID  C STIME TTY          TIME CMD
user      1234     1  0 10:21 ?        00:00:01 python3 main.py
```