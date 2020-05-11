### Apache

**Install:**
```
$ sudo apt update
$ sudo apt install apache2
```

**Start:**
```
$ sudo service apache2 start
```

**Stop:**
```
$ sudo service apache2 stop
```

**Перезагрузить:**
```
$ systemctl reload apache2 
```

**Статус:**
```
$ systemctl status apache2.service 
```

**Чтобы создать локальный сайт mysite.comp:**
-   в директории /var/www создаем каталог 'mysite.comp' в котором должен лежать 'index.html';
-   в директории /etc/apache2/sites-avialable создаем файл 'mysite.comp.conf' (описание страницы) путем копирования файла 000-default.conf:
```
$ sudo cp 000-default.conf mysite.comp.conf;
```
- в файле mysite.comp.conf добавляем следующие настройки:
```
ServerName mysite.comp
DocumentRoot /var/www/mysite.comp
```
- подключаем виртуальный хост:
```
$ sudo a2ensite mysite.comp
```
- в файле etc/hosts связываем свой IP c именем mysite.comp:
```
$ sudo a2ensite mysite.comp
```
- перезагружаем apache:
```
$ systemctl reload apache2 
```


**Чтобы удалить локальный сайт mysite.comp:**
- удаляем все вышесозданные папки и файлы;
- удаляем привязку в etc/hosts;
- отключаем виртуальный хост:
```
$ sudo a2dissite mysite.comp
```
- перезагружаем apache:
```
$ systemctl reload apache2 
```


