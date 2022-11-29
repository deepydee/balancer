# Traefik + Portainer + Nginx

## Перед установкой
Необходимо удалить все старые контейнеры nginx и traefik
```sh
cd ./nginx
docker-compose down
```
```sh
cd ./traefik
docker-compose down
```

## Порядок запуска локального балансировщика

```sh
docker-compose pull
docker-compose up -d
```

## Проекты с nginx-proxy

Для обратной совместимости со старыми проектами, был сохранён nginx. 
Что-бы проект со старой архитектурой заработал, необходимо поправить 78 строку файла docker-compose.yml, добавить нужные нам URL.
```
      - traefik.http.routers.nginx.rule=Host(`sg.test`) || Host(`pma.sg.test`)
```
```
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
