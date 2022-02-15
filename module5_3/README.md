### Задание 1  
https://hub.docker.com/repository/docker/pavlovob/nginx

### Задание 2
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"
Сценарии:  
Высоконагруженное монолитное java веб-приложение  
- Docker в данном случае вряд ли подойдет. Т.к. java приложения как правило, громоздкие, требуют установки библиотек и зависимостей, то тут лучше подойдет виртуальная машина.  
Nodejs веб-приложение  
- На сколько в курсе, Node.js приложения основываются на микросервисной архитектуре и разрабатываются для одновременного выполнения большого кол-во JavaScript запросов (могу путать), поэтому, думаю, Docker здесь как нельзя кстати из-за масштабируемости и получения стабильных разультатов работы экземпляров серверной части.  
Мобильное приложение c версиями для Android и iOS;  
- Если имеется в виду архитекутура MobileApp - API (Rest) - DB, а не простое веб приложение, то скорее всего подойдет Docker, т.к. мобильные приложения так же характеризуются большим кол-вом запросов и масштабирование играет важную роль.  
Шина данных на базе Apache Kafka;
Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
Мониторинг-стек на базе Prometheus и Grafana;
MongoDB, как основное хранилище данных для java-приложения;
Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.
### Задание 3  
```
user@wbn04pavlov:~/netology/docker$ sudo docker pull centos
Using default tag: latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
user@wbn04pavlov:~/netology/docker$ sudo docker pull debian
Using default tag: latest
latest: Pulling from library/debian
0c6b8ff8c37e: Pull complete 
Digest: sha256:fb45fd4e25abe55a656ca69a7bef70e62099b8bb42a279a5e0ea4ae1ab410e0d
Status: Downloaded newer image for debian:latest
docker.io/library/debian:latest
ser@wbn04pavlov:~/netology/docker$ sudo docker images
REPOSITORY       TAG       IMAGE ID       CREATED             SIZE
pavlovob/nginx   1.0.4     bc7b62d6d98b   About an hour ago   134MB
debian           latest    04fbdaf87a6a   2 weeks ago         124MB
centos           latest    5d0da3dc9764   5 months ago        231MB
user@wbn04pavlov:~/netology/docker$ sudo docker run -v /data:/data --name centos -d centos:latest sleep infinity
d99516dcdee6ebc5acd08ce1357addd3f8754490e05e904a7de2e8bc4d38afad
user@wbn04pavlov:~/netology/docker$ sudo docker run -v /data:/data --name debian -d debian:latest sleep infinity
ed3b7136b8becf6de8bc9a21974bf9c9835239e483909a35ac0ed3cb10cc1afd
user@wbn04pavlov:~/netology/docker$ sudo docker ps -a
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                   NAMES
ed3b7136b8be   debian:latest          "sleep infinity"         6 seconds ago    Up 5 seconds                                            debian
d99516dcdee6   centos:latest          "sleep infinity"         31 seconds ago   Up 30 seconds                                           centos
918c266f80f3   pavlovob/nginx:1.0.4   "/docker-entrypoint.…"   2 hours ago      Up 2 hours      0.0.0.0:8080->80/tcp, :::8080->80/tcp   sweet_ellis
user@wbn04pavlov:~/netology/docker$ sudo docker exec -it centos sh
sh-4.4# echo This is CentOS text file! > /data/sample.txt
sh-4.4# cat /data/sample.txt
This is CentOS text file!
sh-4.4# exit
exit
user@wbn04pavlov:~/netology/docker$ echo This is a Host PC text file! | sudo tee -a /data/hostfile.txt
This is a Host PC text file!
user@wbn04pavlov:~/netology/docker$ sudo docker exec -it debian sh
# ls /data
hostfile.txt  sample.txt
# exit
user@wbn04pavlov:~/netology/docker$ 
```
### Задание 4  
```
https://hub.docker.com/repository/docker/pavlovob/ansible
```
