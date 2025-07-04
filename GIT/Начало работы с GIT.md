#git

### Настройка имени и email (локальный логин в Git)
Это нужно, что бы **Git** знал кто ты при коммитах:
```bash
git config --global user.name "Твоё Имя"
git config --global user.email "you@example.com"
```

### Аутентификация через SSH
```bash
ssh-keygen -t ed25519 -C "you@example.com"
```
Потом терминал предложит ввести **path** и имя, после этого он попросит пароль два раза:
![[Pasted image 20250703125014.png]]
Видим, что в нужной директории появились файлы:
![[Pasted image 20250703125217.png]]Далее скопируем ключ из файла `ACC_helper.pub`:
```bush
cat ACC_helper.pub
```
Теперь добавим SSH ключ в GitHub:
- Зайди на [github.com](https://github.com)
- Профиль → Settings → SSH and GPG keys → **New SSH key**

Добавим ключ в ssh-agent, для этого передадим в команду `ssh_add` непубличную часть ключа:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/ACC_helper
```

Ну и проверим подключение:
```bash
ssh -T git@github.com
```

- [[Команды git]]