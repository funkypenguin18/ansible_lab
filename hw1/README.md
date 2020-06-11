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

#### Установка проверялась на ВМ с CentOS Linux release 7.7.1908 (minimal),
адрес ВМ 10.4.160.245 , команда:
```bash
ansible-playbook -u root -i hosts hw1.yaml
  ```

### В процессе выполнения ДЗ:

1. Создана роль, устанавливающая Apache web server на дистрибутивы центос/дебиан
1.1 создан /var/www/html/index.html с текстом “Welcome to my
web server”
Проверка:
```bash
curl http://10.4.160.245/
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
