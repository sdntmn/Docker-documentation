# Docker-in-russian

### Что использовал

Официальная ссылка на документацию [Docker](https://docs.docker.com/)

Видео на ютуб от Владилена Минина [Docker для начинающих ](https://www.youtube.com/watch?v=n9uCgUzfeRQ&t=2534s) и
[PDF с инструкциями](https://vladilen.notion.site/Docker-2021-a72201ec8573461c8a2e62e2fcf33aa3)

Учебник. Создание приложения Docker от Microsoft [с помощью Visual Studio Code](https://learn.microsoft.com/ru-ru/visualstudio/docker/tutorials/docker-tutorial)

## Кратко о Docker

**Docker** — технология для создания и управления **Containers** -` контейнерами`.

**Containers** - `это компактное виртуализованное окружение`, которое предоставляет платформу для создания и запуска приложений. Контейнерам не требуется объем и издержки полной операционной системы.
Мы оборачиваем какой то код или приложение в контейнеры для того, чтобы он нам гарантировал одинаковое поведение в разных окружениях. Поэтому мы можем просто брать докер контейнеры и запускать их где угодно, **где есть докер**. `Нам не важно, что это будет за ОС`, его версия. Все поведение будет зафиксировано в контейнере.

**Containers** - запускаются на основе образов **Images**

**Images** - шаблоны, `только для чтения`- _используется для создания контейнеров_, `содержат все необходимое для запуска приложения` - зависимости, конфигурацию, скрипты и двоичные файлы, переменные среды, команду-выполняемую по умолчанию, и другие метаданные.

## Images - образы

Ссылка на заранее заготовленные образы с официального сайта - [images](https://hub.docker.com/)

Ссылка на образ [node](https://hub.docker.com/_/node)

## Команды

_Документация:_ список команд и их описание на оф. сайте [ссылка](https://docs.docker.com/engine/reference/commandline/docker/)

- Просмотр всех возможных команд `docker --help`
- Справка по любой команде `docker ps --help`
- Односимвольные параметры командной строки можно комбинировать, поэтому вместо ввода (например, `docker run -i -t --name test busybox sh`) вы можете писать (`docker run -it --name test busybox sh`)
- Установка логических параметров отличающихся от параметров по умолчанию (например, `docker build --rm=false .` )

_Список команд_

- [Базовые команды](/docs/BaseCommand.md)

**установка образа** (последняя доступная по умолчанию версия):

```
docker pull node
```

**установка** образа **нужной версии**:

```
docker pull node:16.18
```

**просмотр** установленных **images**:

```
docker images
```

```js
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
<none>       <none>    91f60b67e592   37 hours ago   996MB
node         16.18     147af8719073   38 hours ago   911MB
node         latest    35ff1df466e8   9 days ago     991MB
```

- запуск контейнера на основе определенного образа по имени `docker run node`
- запуск контейнера на основе определенного образа по id `docker run 147af8719073`

```js
CONTAINER ID   IMAGE        COMMAND                  CREATED             STATUS             PORTS     NAMES
539e3a116eff   node:16.18   "docker-entrypoint.s…"   About an hour ago   Up About an hour             hardcore_hellman
```
