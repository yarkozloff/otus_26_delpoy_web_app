# Развертывание веб приложения
## Описание ДЗ
Варианты стенда:
- nginx + php-fpm (laravel/wordpress) + python (flask/django) + js(react/angular);
- nginx + java (tomcat/jetty/netty) + go + ruby;
- можно свои комбинации.
Реализации на выбор:
- на хостовой системе через конфиги в /etc;
- деплой через docker-compose.
К сдаче принимается:
- vagrant стэнд с проброшенными на локалхост портами
- каждый порт на свой сайт
- через нжинкс
Формат сдачи ДЗ - vagrant + ansible

Настроить split-dns
- клиент1 - видит обе зоны, но в зоне dns.lab только web1
- клиент2 видит только dns.lab

Дополнительное задание
* настроить все без выключения selinux

Формат сдачи ДЗ - vagrant + ansible

## Среда выполнения:
```
root@yarkozloff:/otus/deploy_app# hostnamectl | grep "Operating System"
  Operating System: Ubuntu 20.04.3 LTS
root@yarkozloff:/otus/deploy_app# vagrant --version
Vagrant 2.2.19
root@yarkozloff:/otus/deploy_app# vboxmanage --version
6.1.26_Ubuntur145957
root@yarkozloff:/otus/deploy_app# ansible --version
ansible [core 2.12.4]
```
Бокс с 
## Подготовка стенда
Дерево проекта:
```
root@yarkozloff:/otus/deploy_app# tree
.
├── 2022-07-31-15-43-27.023-VBoxHeadless-6693.log
├── 401ad560-e62e-4e55-9cbc-772264c9835c
├── project
│   ├── docker-compose.yml
│   ├── nginx-conf
│   │   └── nginx.conf
│   ├── node
│   │   └── test.js
│   └── python
│       ├── Dockerfile
│       ├── manage.py
│       ├── mysite
│       │   ├── asgi.py
│       │   ├── __init__.py
│       │   ├── settings.py
│       │   ├── urls.py
│       │   └── wsgi.py
│       └── requirements.txt
├── prov.yml
└── Vagrantfile

5 directories, 15 files
```
Итого в docker-compose файле имеем следующие контейнеры объединённые в общую сеть app-network:
- Базу банных MySQL
- WordPress
- Nginx
- App

В конфиге nginx перечислены серверы:
- django (на порту 8081)
- node.js (на порту 8082)
- wordpress (на порту 8083)

Основная работа Ansible будет заключаться в устновке Docker и копировании файловов проекта

## vagrant provisioning
После предварительных испытаний разрушаем машину (vagrant halt) и поднимаем заново (vagrant up). Затем провиженим с записью в файл (vagrant provision > provision_log)

## Проверка
http://localhost:8081/
![image](https://user-images.githubusercontent.com/69105791/182040947-7e1507dd-6022-4592-8a19-288cc972fa45.png)

http://localhost:8082/
![image](https://user-images.githubusercontent.com/69105791/182040966-4a49c7d1-39aa-4acb-9b22-fa5a7dbbda47.png)

http://localhost:8082/
![image](https://user-images.githubusercontent.com/69105791/182041003-23e014e3-0852-4d02-b902-405209fd0cd0.png)
