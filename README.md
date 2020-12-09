# docker-local-registry-proxy-cache
### Описание docker-compose
* `REGISTRY_HTTP_TLS_CERTIFICATE` - предназначен для настройки ssl.
* `REGISTRY_HTTP_TLS_KEY` - предназначен для настройки ssl.

##### Использование UI для registry
* `ENV_DOCKER_REGISTRY_HOST` - указываете имя контейнера с registry.
* `ENV_DOCKER_REGISTRY_PORT` - порт на котором запущен registry.

### Запуск compose файла
* Должны быть созданы каталоги: data, certs, config  в директории с compose 
* В `/etc/docker/daemon.json` нужно добавить `"registry-mirrors": ["https://docker-registry.test.ru"]`
* Выполинть команду `systemctl daemon-reload && service docker restart`
* Настроить nginx на хосте. [Пример nginx конфига](https://github.com/v1adislav/docker-local-registry-proxy-cache/blob/main/docker-registry.conf) для registry
* Запустить сервис `docker-compose up -d`

### Проверка cache proxy
* `curl docker-registry.test.ru/v2/_catalog` вернет вам пустой registry вида `{"repositories":[]}`
* Спулим любой образ, например ubuntu, `docker pull ubuntu`
* Проверим наш docker-registry `curl docker-registry.test.ru/v2/_catalog`, уже должен быть там образ, который спулили `{"repositories":["library/ubuntu"]}`