Порядок запуска локального балансировщика

```sh
docker-compose up -d
sudo systemctl disable systemd-resolved
sudo systemctl stop systemd-resolved
docker-compose up -d
```