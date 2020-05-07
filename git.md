### Git

**Install:**
```
$ sudo apt install git
```

**Version:**
```
$ git --version
```

**First settings:**
```
$ git config --global user.name "DmitrijPedan"
```
```
$ git config --global user.email  dmitrijpedan84@gmail.com
```
```
$ git config --list --show-origin
```
**SSH:**
```
$ ssh-keygen -t rsa -b 4096 -C "dmitrijpedan84@gmail.com"
```
```
$ eval "$(ssh-agent -s)"
```
```
$ ssh-add ~/.ssh/id_rsa
```

> Next: copy content from "id_rsa.pub" and paste to GitHub settings
