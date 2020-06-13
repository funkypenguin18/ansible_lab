# ansible - HW 2
### Задание:

1. Создайте плэйбук uninstall-httpd.yml, который удалит httpd с управляемых хостов, удалит файл /var/www/html/index.html и закроет на фаерволе порты, используемые веб-сервером.

2. На одной из управляемых нод создайте файл /tmp/database и напишите плэйбук, который будет устанавливать mariadb-server, если такой файл существует.

3. Напишите плэйбук, который устанавливает и включает FTP (пакет vsftpd), открывает необходимые порты. Определите в переменных необходимые параметры конфигурации ftp-сервера и используйте их в j2-шаблоне для конфига vsftpd.conf:
    - разрешен анонимный доступ и аплоад файлов
    - /var/ftp/pub - настроены необходимые разрешения и соответствующий SELinux контекст
    - SELinux "ftpd_anon_write" boolean - значение "on"


### В процессе выполнения ДЗ:
1. Создан плэйбук uninstall-httpd.yml, который удаляет httpd с указанных хостов(node1), удаляет файл /var/www/html/index.html и закрывает на фаерволе порты, используемые веб-сервером.

2. Создан плэйбук mariadb.yml, проверяющий при помощи stat, есть ли на node1 файл /tmp/database и если такой файл существует, устанавливает и запускает mariadb-server.

3. Создана роль, устанавливающая и включающая FTP (пакет vsftpd) на node2 

- Задание на Jinja2 темплейты:
минимальный конфиг-файл копируется из jinja2-темплейта.

- SELinux:
До выполнения плейбука:
```bash
[vagrant@node2 ~]$ getsebool -a | grep ftpd_anon_write
ftpd_anon_write --> off
```
После выполнения плейбука:
```bash
[vagrant@node2 ~]$ getsebool -a | grep ftpd_anon_write
ftpd_anon_write --> on
```

```bash
[ansible@control hw1]$ ftp node2
Connected to node2 (192.168.50.112).
220 (vsFTPd 3.0.2)
Name (node2:ansible): Anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
227 Entering Passive Mode (192,168,50,112,170,5).
150 Here comes the directory listing.
drwxr-xr-x    2 99
```

C windows машины:
```bash
C:\Users\пк>ftp -A 192.168.50.112
Связь с 192.168.50.112.
220 (vsFTPd 3.0.2)
200 Always in UTF8 mode.
331 Please specify the password.
230 Login successful.
Установлено анонимное соединение для пк@WIN-L9PRUUPFB59
ftp> cd pub
250 Directory successfully changed.
ftp> put c:\git\test.txt
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
ftp: 7 байт отправлено за 0.00 (сек) со скоростью 7000.00 (КБ/сек).
```

# ansible - HW 1

### Задание:

1. Создайте плэйбук, выполняющий установку веб-сервера Apache на
управляемом хосте со следующими требованиями:
- установка пакета httpd;
- включение службы веб-сервера и проверка, что он запущен;
- создание файла /var/www/html/index.html с текстом “Welcome to my
web server”;
- используйте модуль firewalld для того, чтобы открыть необходимые
для работы веб-сервера порты брендмауэра;
2. Отключите службу NetworkManager
Измените файл /etc/default/grub и добавьте параметры net.ifnames=0
и biosdevname=0 в строку, выполняющую загрузку ядра.
Выполните grub2-mkconfig, чтобы записать изменения в
/etc/default/grub
Используйте модуль lineinfile для изменения конфигурациионого
файла

#### Установка проверялась на node1 с CentOS Linux release 7.8.2003,
команда:
```bash
ansible-playbook -i hosts hw1.yaml
  ```

### В процессе выполнения ДЗ:

1. Создана роль, устанавливающая Apache web server на дистрибутивы центос/дебиан
1.1 создан /var/www/html/index.html с текстом “Welcome to my
web server”
Проверка:
```bash
curl node1
<html>
  <body>
    Welcome to my web server
  </body>
</html>
  ```

2. При помощи lineinfile внесены требуемые изменения в /etc/default/grub ,
   grub2-mkconfig выполнен при помощи
  ```bash
  command: grub2-mkconfig -o /boot/grub2/grub.cfg
  ```
