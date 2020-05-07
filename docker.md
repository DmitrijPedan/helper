## Docker commands

**wiew images:**
```
$ docker images
```

**wiew containers:**
```
$ docker ps
```

**create image from Dockerfile (-t - image name; . - from current dir):**
```
$ docker build -t hello-world .
```

### Описание Dockerfile:
```
FROM node:12.16.1

USER node

WORKDIR /app

CMD ["node", "-e", "setInterval(() => {}, 60000); process.on('SIGTERM', () => process.exit(0));"]

ENTRYPOINT ["node", "-e", "setInterval(() => {}, 60000); process.on('SIGTERM', () => process.exit(0));"]
```

 **Описание:** 
- FROM откуда собирать образ (название из docker-hub);
- USER имя пользователь;
- WORKDIR рабочая директория внутри контейнера (название любое);
- CMD and ENTRYPOINT список команд которые нужно выполнить после старта контейнера:
        "node"                                              // запуск ноды
        "-е"                                                //команда JS eval
        "setInterval(() => {}, 60000);                      // периодичность перезапуска
         process.on('SIGTERM', () => process.exit(0));"     // возможность остановить процесс
- разница между CMD and ENTRYPOINT в том что CMD запустит внутри контейнера shell оболочку bin.sh и выполнит команды в ней, а ENTRYPOINT выполнит команды без оболочки;