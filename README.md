
# Кононенко Александр  Домашнее задание к занятию "Система мониторинга Zabbix"

## Задание 1

Установка Zabbix Server с веб-интерфейсом на centos9.

### Процесс выполнения

1. Так как у меня Centos9  то для установки нужноисклчить пакеты Zabbix  в репозитории EPEL
2. Установка PostgreSQL  
3. Установка репозитория Zabbix  
4. Установка Zabbix сервера, веб-интерфейса и агента  
5. Создание и настройка базы данных  
6. Настройка Zabbix сервера  
7. Запуск и включение служб  

### Использованные команды

```bash
# Исключение пакетов
[epel]
...
excludepkgs=zabbix*

# Установка PostgreSQL
dnf install postgresql-server postgresql-contrib -y
postgresql-setup --initdb
systemctl start postgresql
systemctl enable postgresql

# Установка репозитория Zabbix
rpm -Uvh https://repo.zabbix.com/zabbix/7.0/centos/9/x86_64/zabbix-release-latest-7.0.el9.noarch.rpm
dnf clean all

# Установка Zabbix сервера, веб-интерфейса и агента
dnf install zabbix-server-pgsql zabbix-web-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent -y

# Создание базы данных
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

# Импорт начальной схемы и данных
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# Настройка базы данных для Zabbix сервера
# В файле /etc/zabbix/zabbix_server.conf указать:
# DBPassword=zabbix

# Запуск процессов Zabbix сервера и агента, веб-сервера и PHP-FPM
systemctl restart zabbix-server zabbix-agent httpd php-fpm
systemctl enable zabbix-server zabbix-agent httpd php-fpm
```
### скриншоты 



![Скриншот авторизации Zabbix](img/ZabixAd.png)





## Задание 2: Установка Zabbix Agent на хосты

### Сами Хосты
![Скриншот хостов](img/hosts.png)

### Логи Агента
![Скриншот логов](img/logs.png)

### Мониторинг Latest
![Скриншот мониторинга](img/uzly.png)

### Использованные команды

```bash

# Установка репозитория Zabbix
sudo rpm -Uvh https://repo.zabbix.com/zabbix/7.0/rhel/9/x86_64/zabbix-release-latest-7.0.el9.noarch.rpm

# Установка агента
sudo dnf install zabbix-agent -y

# Редактирование конфига
sudo nano /etc/zabbix/zabbix_agentd.conf

# Основные параметры
Server=192.168.200.178
ServerActive=192.168.200.178
Hostname=test_host

# Запуск и автозагрузка
sudo systemctl start zabbix-agent
sudo systemctl enable zabbix-agent

# Проверка связи
zabbix_get -s 192.168.200.170 -p 10050 -k agent.ping

# Просмотр лога
sudo tail -30 /var/log/zabbix/zabbix_agentd.log
```
