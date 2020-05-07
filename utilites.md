### Utilites

**графический диспетчер устройств:**
```
$ sudo apt install hardinfo
```

**монитор системных ресурсов в командной строке:**
```
$ sudo apt install -y htop
```

**Midnight Commander:**
```
$ sudo apt install -y mc
```

**Make создание команд:**
```
$ sudo apt install -y make
```

**Postman:**
```
$ sudo snap install postman
```

**Cdemu (CD/DVD images):**
```
sudo add-apt-repository ppa:cdemu/ppa
sudo apt-get update
sudo apt-get install cdemu-daemon cdemu-client gcdemu
```


**ZSH:**
```
$ apt update
$ apt-get install -y zsh
```
```
$ curl -L http://install.ohmyz.sh | sh
```

Переключить оболочку по умолчанию:
```
$ chsh -s /bin/zsh
```

Перезагрузка:
```
$ shutdown -r 0
```

Переключение оболочек:
```
$ exec bash  
$ exec zsh
```

Если node & npm not found добавить в файл zshrc:
```
export NVM_DIR=~/.nvm
 [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```

Для темы agnoster:
```
$ sudo apt-get install fonts-powerline
```
Отредактировать файл zshrc:
```
THEME="agnoster"
```

