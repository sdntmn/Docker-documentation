# Docker-in-russian

### Что использовал

Официальная ссылка на документацию [Docker](https://docs.docker.com/)

НЕ официальная рускоязычная версия [Dker](https://dker.ru/)

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

**Документация:** список команд и их описание на оф. сайте [ссылка](https://docs.docker.com/engine/reference/commandline/docker/)

- Просмотр всех возможных команд в терминале `docker --help`
- Справка по любой команде (например,`docker ps --help`)
- Односимвольные параметры командной строки можно комбинировать, поэтому вместо ввода (например, `docker run -i -t --name test busybox sh`) вы можете писать (`docker run -it --name test busybox sh`)
- Установка логических параметров отличающихся от параметров по умолчанию (например, `docker build --rm=false .` )

**Список команд** (ru)

- [Базовые команды](/docs/BaseCommand.md)

### Пример создания контейнера для React приложения

### 1. Устанавливаем нужный образ с официального сайта [images](https://hub.docker.com/)

**установка образа** (последняя доступная по умолчанию версия):

```
docker pull node
```

или

**установка** образа **нужной версии**:

```
docker pull node:16.18
```

### Отступление для общего понимания

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

### 2. Создаем Dockerfile

- создание Dockerfile `touch Dockerfile` , ну или ручками в корневой директории

```js
FROM <image>	Определяет основу для вашего образа.
RUN <command>	Выполняет любые команды в новом слое поверх текущего изображения и фиксирует результат. RUNтакже имеет форму оболочки для запуска команд.
WORKDIR <directory>	Задает рабочий каталог для любого RUN, CMD, ENTRYPOINT, COPY, и ADDинструкции, которые следуют за ним в Dockerfile.
COPY <src> <dest>	Копирует новые файлы или каталоги <src>и добавляет их в файловую систему контейнера по пути <dest>.
CMD <command>	Позволяет определить программу по умолчанию, которая запускается при запуске контейнера на основе этого образа. Каждый файл Dockerfile имеет только один CMD, и только последний CMD экземпляр учитывается, когда существует несколько.
```

- пример наполнения Dockerfile:

```js
# Устанавливаем образ(image) который будет использоваться (node:16.18)
FROM node:16.18
# рабочая директория
WORKDIR /app
#копируем файл package.json
COPY package.json .
#устанавливаем зависимости
RUN npm install
# копирование всего
COPY . .
#порт приложения
EXPOSE 3000
# старт приложения
CMD ["npm", "start"]
```

### 3 Создаем образ контейнера на основании Dockerfile

- Создаем образ командой `docker build .` - создается образ без имени, без тега
- Создаем образ командой `docker build --tag react .` - создается образ с именем `react`, с тегом по умолчанию
- Создаем образ командой `docker build --tag react:2.0 .` - создается образ с именем `react`, с тегом по `2.0` ?

  - **Посмотреть** и убедиться в создании образа `docker images`
  - **Посмотреть** запущенные контейнеры `docker ps`
  - **Остановить** работающие контейнеры `docker stop react` - где `react` это имя контейнера или прописываем его ID
  - **Запуск** созданного контейнера `docker start react` - где `react` это имя контейнера или прописываем его ID

### 4 Запускаем приложение - контейнер

- Запускаем контейнер созданного образа `docker run react` - на порту по умолчанию, но при таком запуске мы не сможем увидеть работающее приложение на localhost
- Запускаем контейнер созданного образа `docker run -p 80:3000 react` - выставляем флаг `-p` и прописываем порт 80 или ку на котором хотим работать и 3000 указывает на порт который прописан в докер контейнере
- Если хотим работать в этом же терминале то добавляем флаг `-d` - команда `docker run -d -p 80:3000 react` --detach , -d Запускает контейнер в фоновом режиме и выводит идентификатор контейнера
