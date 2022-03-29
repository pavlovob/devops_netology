## Задача 1

Используя докер образ [centos:7](https://hub.docker.com/_/centos) как базовый и 
[документацию по установке и запуску Elastcisearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/targz.html):  

- составьте Dockerfile-манифест для elasticsearch

содержимое elasticsearch.yml файла:  
```
cluster.name: "docker-cluster"
network.host: 0.0.0.0
node.name: "netology_test"
xpack.security.enabled: false
http.port: 9200
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
```
содержимое Dockerfile:  
```
FROM centos:7
MAINTAINER Oleg_Pavlov
RUN groupadd -g 1000 elasticsearch && useradd elasticsearch -u 1000 -g 1000
RUN yum -y update --nogpgcheck
RUN yum -y install wget gpg java-1.8.0-openjdk.x86_64 && yum clean all
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.9.2-x86_64.rpm --no-check-certificate
RUN rpm -ivh elasticsearch-7.9.2-x86_64.rpm
WORKDIR /usr/share/elasticsearch
RUN set -ex && for path in data logs config config/scripts; do \
        mkdir -p "$path"; \
        chown -R elasticsearch:elasticsearch "$path"; \
    done
COPY elasticsearch.yml /usr/share/elasticsearch/config/
RUN chown -R elasticsearch:elasticsearch /usr/share/elasticsearch
USER elasticsearch
ENV PATH=$PATH:/usr/share/elasticsearch/bin
CMD ["elasticsearch"]
EXPOSE 9200 9300
```
- соберите docker-образ и сделайте `push` в ваш docker.io репозиторий:  

![image](https://user-images.githubusercontent.com/22905019/160661306-fbb2004c-8094-492a-b85d-8b99dabed964.png)

![image](https://user-images.githubusercontent.com/22905019/160661443-8d6da3e4-953e-4039-92d6-7163750d4a0d.png)

![image](https://user-images.githubusercontent.com/22905019/160658872-7dd7869c-f770-4d6a-b9c4-6d4aa87e54d5.png)

[Ссылка на образ](https://hub.docker.com/repository/docker/pavlovob/netology_elastic65)

- запустите контейнер из получившегося образа и выполните запрос пути `/` c хост-машины:  
![image](https://user-images.githubusercontent.com/22905019/160659686-f5c08db5-ad8f-4eb9-9996-8cabeb07d6d0.png)  

![image](https://user-images.githubusercontent.com/22905019/160660496-466a6bd9-4e60-455d-9868-49e037a98b37.png)  

## Задача 2

В этом задании вы научитесь:
- создавать и удалять индексы
- изучать состояние кластера
- обосновывать причину деградации доступности данных

Ознакомтесь с [документацией](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html) 
и добавьте в `elasticsearch` 3 индекса, в соответствии со таблицей:

| Имя | Количество реплик | Количество шард |
|-----|-------------------|-----------------|
| ind-1| 0 | 1 |
| ind-2 | 1 | 2 |
| ind-3 | 2 | 4 |

Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.

Получите состояние кластера `elasticsearch`, используя API.

Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

Удалите все индексы.

**Важно**

При проектировании кластера elasticsearch нужно корректно рассчитывать количество реплик и шард,
иначе возможна потеря данных индексов, вплоть до полной, при деградации системы.

## Задача 3

В данном задании вы научитесь:
- создавать бэкапы данных
- восстанавливать индексы из бэкапов

Создайте директорию `{путь до корневой директории с elasticsearch в образе}/snapshots`.

Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.

**Приведите в ответе** запрос API и результат вызова API для создания репозитория.

Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.

[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.

**Приведите в ответе** список файлов в директории со `snapshot`ами.

Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.

[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 

**Приведите в ответе** запрос к API восстановления и итоговый список индексов.

Подсказки:
- возможно вам понадобится доработать `elasticsearch.yml` в части директивы `path.repo` и перезапустить `elasticsearch`

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
