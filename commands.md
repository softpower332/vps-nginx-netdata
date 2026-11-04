# Журнал: что делал при развёртывании

## VPS
- ОС: Ubuntu Server 22.04.5 LTS. Выбор — стандарт для хостинга, стабильный `apt`.

## Подключение
```bash
ssh root@X.X.X.X
# зачем: удалённое управление сервером. Порт по умолчанию — 22 (sshd).
# аутентификация по паролю. 
```

## Базовые команды
```bash
sudo apt update              # обновить список пакетов из репозиториев
sudo apt install <пакет>     # установить пакет
systemctl status <служба>    # проверить состояние службы
ip a                         # посмотреть IP и сетевые интерфейсы
ufw status                   # результат: inactive (файрвол выключен)
curl -I http://localhost     # проверить, что локальный веб-сервер отвечает
```

## UFW
```bash
ufw status
# результат: inactive. Не настраивал — для учебного проекта оставил как есть.
```

## Nginx — установка и проверка
```bash
sudo apt install nginx 
systemctl status nginx       # служба запущена (active running)
curl -I http://localhost     # ответ: 200 OK
# открыл http://X.X.X.X в браузере — отдалась дефолтная страница «Welcome to nginx!»
```

## Netdata — установка
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
systemctl status netdata
# веб-интерфейс: http://X.X.X.X:19999
```

## Netdata — ограничение доступа (осознанное изменение конфига)
```bash
# Цель: служба не должна быть доступна из интернета, только локально
sudo nano /etc/netdata/netdata.conf
# bind to = 127.0.0.1
sudo systemctl restart netdata
sudo ss -tulpn | grep 19999
# проверка: теперь слушается только 127.0.0.1:19999, снаружи недоступно
```
