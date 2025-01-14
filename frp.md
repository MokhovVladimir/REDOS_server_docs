# Документация RED OS по настройке FTP сервера

dnf provides /usr/sbin/semanage

Вывод формата
```
Последняя проверка окончания срока действия метаданных: 1:35:43 назад, Пн 13 янв 2025 23:45:06.
policycoreutils-python-utils-3.4-7.red80.noarch : SELinux policy core python utilities
Репозиторий        : base
Совпадения с:
Имя файла   : /usr/sbin/semanage

policycoreutils-python-utils-3.5-1.red80.noarch : SELinux policy core python utilities
Репозиторий        : @System
Совпадения с:
Имя файла   : /usr/sbin/semanage

policycoreutils-python-utils-3.5-1.red80.noarch : SELinux policy core python utilities
Репозиторий        : updates
Совпадения с:
Имя файла   : /usr/sbin/semanage
```

В случае нахождения пакетов их необходимо установить

dnf install vsftpd ftp nano policycoreutils-python-utils

systemctl enable vsftpd --now

cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.backup

Далее приведен пример файла /etc/vsftpd/vsftpd.conf

```
...
anonymous_enable=NO
user_config_dir=/etc/vsftpd_user_conf
pasv_enable=YES


local_enable=YES
write_enable=YES

chroot_local_user=YES
allow_writeable_chroot=YES
...
```
## Добавление пользователя для работы с FTP сервером
useradd user1 -d /home/user1

passwd user1

usermod -aG ftp user1

mkdir /etc/vsftpd_user_conf

Пример файла /etc/vsftpd_user_conf/user1
```
local_root=/srv/ftp/
```


mkdir /srv/ftp
chown -R user1:user1 /srv/ftp

## Измение настроек SE-Linux
semanage fcontext -a -t public_content_rw_t "/srv/ftp(/.*)?"

semanage fcontext -a -t httpd_sys_content_t "/srv/ftp/(/.*)?"

chcon -R -t httpd_sys_rw_content_t /srv/ftp/

chcon -R -t public_content_rw_t /srv/ftp/

setsebool -P tftp_home_dir on

setsebool -P ftpd_full_access on

restorecon -Rv /srv/ftp


systemctl restart vsftpd