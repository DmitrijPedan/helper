## Docker

**Посмотреть список локальных images:**
```$ docker images```

**Создать image из Dockerfile с именем 'image-name' (-t - имя (тэг); . из текущей директории):**
*если повторно собрать этот образ с с таким же именем то в списке образов появится образ без имени (это предыдущий образ)*
```$ docker build -t image-name .```

**Запустить image (создать контейнер) из образа 'image-name':**
```$ docker run image-name```

**Удалить image (параметром нужно передать имя или ID):**
```$ docker rmi image-name```

**Удалить все image (параметром нужно передать список ID):**
```$ docker rmi $(docker images -q)```

**Дополнительные ключи для docker run:** 
- --name container-name // присвоить контейнеру имя container-name, если не указать - Docker даст свое имя;
- -d    // запустить в фоне (не занимать терминал);
- --rm  // удалить контейнер после завершения работы (если контейнер работает бесконечно (например сервер) то он удалится только после команды 'docker stop');
- -p 8080:8080  // пробросить порт наружу (порт машины : порт контейнера);
- -e TZ=Europe/Kiev // установить переменной окружения TZ значение Europe/Kiev (часовой пояс внутри контейнера);
- -v /home/dima/projects/myapp/db:/usr/src/myapp/db   //прикрутить директорию снаружи (например для БД) (абсолютный путь на машине : абсолютный путь контейнера);
- -v db_myapp:/usr/src/myapp/db   //прикрутить volume db_myapp к директории /usr/src/myapp/db контейнера;

**Например запустить image (создать контейнер) с именем 'container-name', удалить контейнер после завершения, пробросить порт, задать часовой пояс, пробросить директорию:**
```$ docker run --name container-name -d --rm -p 8080:8080 -e TZ=Europe/Kiev -v /home/dima/projects/myapp/db:/usr/src/myapp/db image-name```

**Посмотреть работающие контейнеры:**
```$ docker ps```

**Посмотреть все контейнеры (в т.ч. те которые уже отработали):**
```$ docker ps -a```

**Посмотреть все контейнеры и вернуть только их ID:**
```$ docker ps -a -q```

**Остановить контейнер (параметром нужно передать имя контейнера или его ID):**
```$ docker stop container-name```
```$ docker stop 1e0c7ccd00041```

**Удалить контейнер (параметром нужно передать имя контейнера или его ID):**
```$ docker rm container-name```
```$ docker rm 1e0c7ccd00041```

**Удалить все контейнеры по ID (параметром передаем список ID результат выполнения docker ps -a -q):**
```$ docker rm $(docker ps -a -q)```

**Создать volume 'db_myapp' (volume это обычная директория в системе которую можно присоединить к любому контейнеру):**
```$ docker volume create db_myapp```

**Посмотреть список всех volumes в системе:**
```$ docker volume ls```

### Описание Dockerfile:
```
FROM node:12.16.1
USER node
RUN mkdir -r /usr/src/myapp
WORKDIR /usr/src/myapp
COPY . /usr/src/myapp
RUN npm install
EXPOSE 8080 
ENV TZ Europe/Kiev
CMD ["node", "-e", "setInterval(() => {}, 60000); process.on('SIGTERM', () => process.exit(0));"]
ENTRYPOINT ["node", "-e", "setInterval(() => {}, 60000); process.on('SIGTERM', () => process.exit(0));"]
```

 **Описание:** 
 *Dockerfile отрабатывает один раз при сборке*
- FROM откуда собирать образ (название из docker-hub);
- USER имя пользователя;
- RUN mkdir -r /usr/src/myapp; //создать директорию внутри контейнера
- WORKDIR установить рабочую директорию внутри контейнера;
- COPY . /usr/src/myapp; скопировать содержимое текущей директории (относительно Dockerfile) в заданную директорию в контейнере;
- RUN выполнить команду (например установить все зависимости package.json);
- EXPOSE 8080; // задекларировать (не пробросить) порт;
- ENV TZ Europe/Kiev; // установить переменной окружения TZ значение Europe/Kiev (часовой пояс внутри контейнера);
- CMD and ENTRYPOINT список команд которые нужно выполнить после старта контейнера:
- *"node"  // запуск ноды;*
- *"-е"  //команда JS eval;*
- *"setInterval(() => {}, 60000); process.on('SIGTERM', () => process.exit(0));" // периодичность перезапуска; возможность остановить процесс;*
- *разница между CMD and ENTRYPOINT в том что CMD запустит внутри контейнера shell оболочку bin.sh и выполнит команды в ней, а ENTRYPOINT выполнит команды без оболочки;*

## docker-compose

### Описание dicker-compose.yaml:
```
version: '3.6'

volumes: 
    myapp_db:

services: 
    node:
        build: node/
        restart: unless-stopped (always)
        ports: 
            - "80:3000"
        environment:
            - TZ=Europe/Kiev
            - DB_ADDR=db
            - DB_PORT=5454
            - LANGUAGE=RU
            - MY_USE_URL='https://dp.io'
    db:
        image: image-name
        restart: unless-stopped (always)
        volumes: 
            - myapp_db:/data/db
```

 **Описание:** 
- version: '3.6' // версия парсера;
- volumes: // используемые volumes;
- services: // контейнеры;
    - node: //имя сервиса (контейнера);
        - build: docker/node; // путь к Dockerfile;
        - restart: unless-stopped; // параметры рестарта
        - ports: - "8080:3000"; // проброс портов (снаружи:внутри);
        - volumes: - "./client:/www"; // прикрутить volume ./client к директории /www контейнера
        - environment: // задать значения переменным окружения в контейнере:
            - DB_ADDR=db // адрес БД (имя следующего сервиса db) при запуске контейнеров node и db по имени 'db' будет доступен контейнер с БД;
    - db: //имя сервиса (контейнера);
        - image: image-name; // собрать контейнер с образа image-name;
        - volumes: - myapp_db:/data/db; // прикрутить к папке '/data/db' контейнера volume 'myapp_db';
