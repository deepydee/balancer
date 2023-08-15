# Traefik + Portainer + Nginx + Mailpit

## 1. Перед установкой

Необходимо удалить все старые контейнеры nginx и traefik

```sh
cd ./nginx
docker-compose down -v
```

```sh
cd ./traefik
docker-compose down -v
```

## 2. Порядок запуска локального балансировщика

```sh
docker-compose pull
docker-compose up -d
```

## 3. Использование Mailpit (локальный сборщик почты SMTP)

Подробнее про Mailpit можно почитать тут <https://github.com/axllent/mailpit>.

Для входа в веб-интерфейс Mailpit и просмотра перехваченных писем используйте адрес <http://mailpit.test> (добавьте его в /etc/hosts предварительно).

### Используйте следующие параметры на любом бэкенде

- Способ отправки - SMTP
- Сервер SMTP - mailpit.test
- Порт SMTP - 1025

Больше никаких данных не нужно! Поля с логином, паролем и типом шифрования оставляйте пустыми.

## 4. Проекты с nginx-proxy

Для обратной совместимости со старыми проектами, был сохранён nginx.
Что-бы проект со старой архитектурой заработал, необходимо поправить 78 строку файла docker-compose.yml, добавить нужные нам URL.

```sh
- traefik.http.routers.nginx.rule=Host(`sg.test`) || Host(`pma.sg.test`)
```

```sh
- traefik.http.routers.nginx.rule=Host(`sg.test`) || Host(`pma.sg.test`) || Host(`oop.test`) || Host(`pma.oop.test`)
```

После изменения строки нужно пересоздать контейнер, что-бы применить изменения.

```sh
docker-compose up -d
```

Иногда бывает что контейнер с nginx не может обнаружить контейнер с нужным адресом, для этого необходимо выполнить команду

```sh
docker-compose up -d --force-recreate nginx
```
