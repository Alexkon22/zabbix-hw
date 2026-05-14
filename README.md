# Домашнее задание к занятию "Система мониторинга Zabbix" - Кононенко Александр

---

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

**Процесс выполнения:**

1. Установка PostgreSQL
2. Установка репозитория Zabbix
3. Установка Zabbix сервера, веб-интерфейса и агента
4. Создание и настройка базы данных
5. Настройка Zabbix сервера
6. Запуск и включение служб

**Использованные команды:**

```bash
# Установка PostgreSQL
dnf install postgresql-server postgresql-contrib
postgresql-setup --initdb
systemctl start postgresql
systemctl enable postgresql

# Исключение пакетов Zabbix из EPEL (если установлен)
# Отредактировать /etc/yum.repos.d/epel.repo и добавить:
# [epel]
# excludepkgs=zabbix*

# Установка репозитория Zabbix
rpm -Uvh https://repo.zabbix.com/zabbix/7.0/centos/9/x86_64/zabbix-release-latest-7.0.el9.noarch.rpm
dnf clean all

# Установка Zabbix сервера, веб-интерфейса и агента
dnf install zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent

# Создание базы данных
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

# Импорт начальной схемы и данных
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# Настройка базы данных для Zabbix сервера
# Отредактировать /etc/zabbix/zabbix_server.conf
# DBPassword=password

# Запуск процессов Zabbix сервера и агента
systemctl restart zabbix-server zabbix-agent httpd php-fpm
systemctl enable zabbix-server zabbix-agent httpd php-fpm
###Скриншот 

![Авторизация в Zabbix](https://raw.githubusercontent.com/Alexkon22/zabbix-hw/main/img/ZabixAd.png)

