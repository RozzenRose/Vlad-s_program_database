#docker #ghcr

Это хранилище `docker` - образов от `Git`.

Для того, что бы им воспользоваться заходим в настройки на сайте `github.com`, снизу слева находим `Developers settings`:
![[Pasted image 20251226141112.png]]
![[Pasted image 20251226141127.png]]

Генерируем новый токен:
![[Pasted image 20251226141159.png]]
ставим галку с пермитом


После этого логинемся в `ghcr` на на машине, с которой будем делать пуши:
```
docker login ghcr.io -u rozzenrose -p YOUR_TOKEN
```

В случае, если нам вдруг надо поменять токен на компьютере, на котором он закончился, необходимо разлогинится:
```bash
docker logout
```
Затем сбросить кеш:
```bash
docker logout ghcr.io
rm -rf ~/.docker/config.json
```
А уже затем логинимся снова:
```bash
echo "<YOUR_TOKEN>" | docker login ghcr.io -u <YOUR_GITHUB_USERNAME> --password-stdin
```