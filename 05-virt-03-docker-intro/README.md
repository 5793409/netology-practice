
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .
## --- Ответ ---
```bash
https://hub.docker.com/repository/docker/wisebeard/custom-nginx/general
```


## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
## --- Ответ ---
```bash
root@fedora ~ $ docker run -d -p 127.0.0.1:8080:80 --name Nikolay_Kovtykh-custom-nginx-t2 custom-nginx:1.0.0
b0ff154887785df721660461e5e0a0ffd044b843c605bb22e6bc2c93866df910
root@fedora ~ $
root@fedora ~ $ docker container rename Nikolay_Kovtykh-custom-nginx-t2 custom-nginx-t2
root@fedora ~ $
root@fedora ~ $ date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
22-10-2024 12:02:42.399203580 MSK
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                    NAMES
b0ff15488778   custom-nginx:1.0.0   "/docker-entrypoint.…"   55 seconds ago   Up 54 seconds   127.0.0.1:8080->80/tcp   custom-nginx-t2
LISTEN 0      4096       127.0.0.1:8080       0.0.0.0:*    users:(("docker-proxy",pid=34859,fd=4))  
2024/10/22 09:01:48 [notice] 1#1: start worker process 34
PGh0bWw+Cgo8aGVhZD4KICAgIEhleSwgTmV0b2xvZ3kKPC9oZWFkPgoKPGJvZHk+CiAgICA8aDE+
SSB3aWxsIGJlIERldk9wcyBFbmdpbmVlciE8L2gxPgo8L2JvZHk+Cgo8L2h0bWw+
root@fedora ~ $
root@fedora ~ $ curl http://localhost:8080
<html>

<head>
    Hey, Netology
</head>

<body>
    <h1>I will be DevOps Engineer!</h1>
</body>

</html>root@fedora ~ $ 


```


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
## --- Ответ ---
```bash
root@fedora ~ $ docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS         PORTS                    NAMES
b0ff15488778   custom-nginx:1.0.0   "/docker-entrypoint.…"   12 minutes ago   Up 8 seconds   127.0.0.1:8080->80/tcp   custom-nginx-t2
root@fedora ~ $ docker attach custom-nginx-t2 
^C2024/10/22 09:14:46 [notice] 1#1: signal 2 (SIGINT) received, exiting
2024/10/22 09:14:46 [notice] 25#25: exiting
2024/10/22 09:14:46 [notice] 24#24: exiting
2024/10/22 09:14:46 [notice] 24#24: exit
2024/10/22 09:14:46 [notice] 25#25: exit
2024/10/22 09:14:46 [notice] 26#26: exiting
2024/10/22 09:14:46 [notice] 26#26: exit
2024/10/22 09:14:46 [notice] 27#27: exiting
2024/10/22 09:14:46 [notice] 27#27: exit
2024/10/22 09:14:46 [notice] 1#1: signal 17 (SIGCHLD) received from 27
2024/10/22 09:14:46 [notice] 1#1: worker process 27 exited with code 0
2024/10/22 09:14:46 [notice] 1#1: signal 29 (SIGIO) received
2024/10/22 09:14:46 [notice] 1#1: signal 17 (SIGCHLD) received from 24
2024/10/22 09:14:46 [notice] 1#1: worker process 24 exited with code 0
2024/10/22 09:14:46 [notice] 1#1: worker process 25 exited with code 0
2024/10/22 09:14:46 [notice] 1#1: signal 29 (SIGIO) received
2024/10/22 09:14:46 [notice] 1#1: signal 17 (SIGCHLD) received from 26
2024/10/22 09:14:46 [notice] 1#1: worker process 26 exited with code 0
2024/10/22 09:14:46 [notice] 1#1: exit
root@fedora ~ $ 

# через docker attach custom-nginx-t2, присоединились к потоку ввода/вывода контейнера "custom-nginx-t2",
# комбинация Ctrl-C отправила сигнал завершения работы рабочего процесса,
# в контейнере работал только nginx, его работа была завершена, контейнер выключился. 


root@fedora ~ $ docker container restart custom-nginx-t2 
custom-nginx-t2
root@fedora ~ $ docker exec -it custom-nginx-t2 bash
root@b0ff15488778:/# apt update
root@b0ff15488778:/# apt install vim
root@b0ff15488778:/# vim /etc/nginx/conf.d/default.conf 
root@b0ff15488778:/# nginx -s reload
2024/10/22 09:19:49 [notice] 423#423: signal process started
root@b0ff15488778:/# curl http://127.0.0.1:80 ; curl http://127.0.0.1:81
curl: (7) Failed to connect to 127.0.0.1 port 80: Connection refused
<html>

<head>
    Hey, Netology
</head>

<body>
    <h1>I will be DevOps Engineer!</h1>
</body>

</html>root@b0ff15488778:/# exit
root@fedora ~ $ 
root@fedora ~ $ ss -tlpn | grep 127.0.0.1:8080
LISTEN 0      4096       127.0.0.1:8080       0.0.0.0:*    users:(("docker-proxy",pid=35845,fd=4))  
root@fedora ~ $ docker port custom-nginx-t2
80/tcp -> 127.0.0.1:8080
root@fedora ~ $ curl http://127.0.0.1:8080
curl: (56) Recv failure: Соединение разорвано другой стороной

# суть проблемы - nginx слушает входящие соедиения на 81 порту вместо 80,
# настройки контейнера при создании были указаны явно, пробросить порт хоста 8080 на 80 контейнера (-p 127.0.0.1:8080:80) 

# задача - сделать PortMapping 8080:81


# посмотрим id контейнера
root@fedora ~ $ docker inspect --format="{{.Id}}" custom-nginx-t2 
b0ff154887785df721660461e5e0a0ffd044b843c605bb22e6bc2c93866df910
root@fedora ~ $ ls /var/lib/docker/containers/
b0ff154887785df721660461e5e0a0ffd044b843c605bb22e6bc2c93866df910

# остановим запущенный контейнер
root@fedora ~ $ docker container stop custom-nginx-t2 
custom-nginx-t2

# и службу docker на хосте
root@fedora ~ $ systemctl stop docker.service docker.socket

# редактируем настройки контейнера
root@fedora ~ $ vim /var/lib/docker/containers/b0ff154887785df721660461e5e0a0ffd044b843c605bb22e6bc2c93866df910/hostconfig.json
# вносим изменения в  "PortBindings": {"81/tcp":[{"HostIp":"127.0.0.1","HostPort":"8080"}]},

vim /var/lib/docker/containers/b0ff154887785df721660461e5e0a0ffd044b843c605bb22e6bc2c93866df910/config.v2.json
# вносим изменения в  "ExposedPorts":{"81/tcp":{}},

# стратуем сервис докера и контейнер
root@fedora ~ $ systemctl start docker.service docker.socket
root@fedora ~ $ docker container start custom-nginx-t2

# проверяем, успешно 127.0.0.1:8080->81/tcp
root@fedora ~ $ docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                    NAMES
b0ff15488778   custom-nginx:1.0.0   "/docker-entrypoint.…"   41 minutes ago   Up 21 seconds   127.0.0.1:8080->81/tcp   custom-nginx-t2

# Удалите запущенный контейнер "custom-nginx-t2", не останавливая его
root@fedora ~ $ docker container rm --force custom-nginx-t2 

# проверяем
root@fedora ~ $ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

root@fedora ~ $ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES


```

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
## --- Ответ ---
```bash
root@fedora /tmp/netology $ docker run -it --rm -d --name cent_srv1 -v $(pwd):/data centos:7
Unable to find image 'centos:7' locally
7: Pulling from library/centos
2d473b07cdd5: Pull complete 
Digest: sha256:be65f488b7764ad3638f236b7b515b3678369a5124c47b8d32916d6487418ea4
Status: Downloaded newer image for centos:7
5f8a7ee068ad4f86ee2b7f268193edc476bdcf907aafa8b2d84da8a326f8f507

root@fedora /tmp/netology $ docker run -it --rm -d --name debian_srv1 -v $(pwd):/data debian:latest
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
7d98d813d54f: Pull complete 
Digest: sha256:e11072c1614c08bf88b543fcfe09d75a0426d90896408e926454e88078274fcb
Status: Downloaded newer image for debian:latest
a3fb250fdea38ee8fc91d8c05d4716b40a3d94fea694f845a3c1ac19e3d7aea7

root@fedora /tmp/netology $ docker exec -it cent_srv1 bash
[root@5f8a7ee068ad /]# echo "This file created from docker cent_srv1" >> /data/cent_srv1.txt
[root@5f8a7ee068ad /]# exit

root@fedora /tmp/netology $ echo "This file created from host" >> host.txt
root@fedora /tmp/netology $ docker exec -it debian_srv1 bash

root@a3fb250fdea3:/# ls /data
cent_srv1.txt  host.txt
root@a3fb250fdea3:/# more /data/*.txt | cat
::::::::::::::
/data/cent_srv1.txt
::::::::::::::
This file created from docker cent_srv1
::::::::::::::
/data/host.txt
::::::::::::::
This file created from host
root@a3fb250fdea3:/# 
exit
root@fedora /tmp/netology $ 

```

## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.


## --- Ответ ---
```bash
root@fedora /tmp/netology/docker/task5 $ cat <<EOF > compose.yaml
> version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
> EOF

root@fedora /tmp/netology/docker/task5 $ cat <<EOF > docker-compose.yaml
> version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
> EOF


root@fedora /tmp/netology/docker/task5 $ 
root@fedora /tmp/netology/docker/task5 $ docker compose up -d
WARN[0000] Found multiple config files with supported names: /tmp/netology/docker/task5/compose.yaml, /tmp/netology/docker/task5/docker-compose.yaml 
WARN[0000] Using /tmp/netology/docker/task5/compose.yaml 
WARN[0000] /tmp/netology/docker/task5/compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 12/12
 ✔ portainer Pulled                                                                                                                                                                     17.3s 
   ✔ 2fdd3e02e7e5 Pull complete                                                                                                                                                          0.7s 
   ✔ 3745b0e5e59c Pull complete                                                                                                                                                          0.7s 
   ✔ d40df14c1d7a Pull complete                                                                                                                                                          4.9s 
   ✔ 8215717c7c10 Pull complete                                                                                                                                                          8.8s 
   ✔ 542669febe7c Pull complete                                                                                                                                                          9.1s 
   ✔ 6c27c7f45b54 Pull complete                                                                                                                                                          9.5s 
   ✔ 8acaeb74c182 Pull complete                                                                                                                                                          9.6s 
   ✔ c6e890230c95 Pull complete                                                                                                                                                         14.1s 
   ✔ bb1e24a5d5a1 Pull complete                                                                                                                                                         14.4s 
   ✔ 4350eaf0c914 Pull complete                                                                                                                                                         14.4s 
   ✔ 4f4fb700ef54 Pull complete                                                                                                                                                         14.4s 
[+] Running 2/2
 ✔ Container task5-portainer-1                                          Started                                                                                                          0.9s 
 ! portainer Published ports are discarded when using host network mode                                                                                                                  0.0s 

# Какой из файлов был запущен и почему? - по канонам файл compose.yaml имеет больший приоритет

# добавим в основной файл compose.yaml 
# include:
#  - docker-compose.yaml

root@fedora /tmp/netology/docker/task5 $ vim compose.yaml
root@fedora /tmp/netology/docker/task5 $ 
root@fedora /tmp/netology/docker/task5 $ 
root@fedora /tmp/netology/docker/task5 $ docker compose up -d
WARN[0000] Found multiple config files with supported names: /tmp/netology/docker/task5/compose.yaml, /tmp/netology/docker/task5/docker-compose.yaml 
WARN[0000] Using /tmp/netology/docker/task5/compose.yaml 
WARN[0000] /tmp/netology/docker/task5/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 6/6
 ✔ registry Pulled                                                                                                                                                                       5.3s 
   ✔ 1cc3d825d8b2 Pull complete                                                                                                                                                          1.7s 
   ✔ 85ab09421e5a Pull complete                                                                                                                                                          1.8s 
   ✔ 40960af72c1c Pull complete                                                                                                                                                          2.1s 
   ✔ e7bb1dbb377e Pull complete                                                                                                                                                          2.1s 
   ✔ a538cc9b1ae3 Pull complete                                                                                                                                                          2.4s 
[+] Running 3/3
 ✔ Container task5-portainer-1                                         Running                                                                                                           0.0s 
 ✔ Container task5-registry-1                                          Started                                                                                                           0.3s 
 ! registry Published ports are discarded when using host network mode   

```

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


