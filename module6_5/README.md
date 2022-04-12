## Задача 1

текст Dockerfile манифеста

```
FROM centos:7
MAINTAINER Oleg_Pavlov
RUN groupadd -g 1000 elasticsearch && useradd elasticsearch -u 1000 -g 1000
RUN mkdir /var/lib/elasticsearch && mkdir /var/log/elasticsearch
COPY elasticsearch.yml /etc/elasticsearch/
RUN yum -y update --nogpgcheck
RUN yum -y install wget gpg java-1.8.0-openjdk.x86_64 && yum clean all
RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.9.2-x86_64.rpm --no-check-certificate
RUN rpm -ivh elasticsearch-7.9.2-x86_64.rpm
RUN chown -R elasticsearch:elasticsearch /var/lib/elasticsearch && \
    chown -R elasticsearch:elasticsearch /var/log/elasticsearch && \
    chown -R elasticsearch:elasticsearch /usr/share/elasticsearch && \
    chown -R elasticsearch:elasticsearch /etc/elasticsearch/
WORKDIR /usr/share/elasticsearch
EXPOSE 9200 9300
USER elasticsearch
ENV PATH=$PATH:/usr/share/elasticsearch/bin
CMD ["elasticsearch"]
```
ссылка на образ в репозитории dockerhub:

[Образ на Dockerhub](https://hub.docker.com/r/pavlovob/esi)

ответ elasticsearch на запрос пути / в json виде:

![image](https://user-images.githubusercontent.com/22905019/163001487-6f555fac-27e2-4a16-8b52-72ab91a66165.png)

![image](https://user-images.githubusercontent.com/22905019/160901532-2a5fbfbe-cddc-4f67-abcb-182e26964991.png)

## Задача 2

добавьте в elasticsearch 3 индекса, в соответствии со таблицей:

| Имя | Количество реплик | Количество шард |
|-----|-------------------|-----------------|
| ind-1| 0 | 1 |
| ind-2 | 1 | 2 |
| ind-3 | 2 | 4 |

```
[elasticsearch@3e58a67ff06c elasticsearch]$ curl -X PUT "localhost:9200/ind-1?pretty" -H 'Content-Type: application/json' -d'
> {
>   "settings": {
>     "number_of_shards": 1,
>     "number_of_replicas": 0
>   }
> }
> '
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "ind-1"
}
[elasticsearch@3e58a67ff06c elasticsearch]$ curl -X PUT "localhost:9200/ind-2?pretty" -H 'Content-Type: application/json' -d'
> {
>   "settings": {
>     "number_of_shards": 2,
>     "number_of_replicas": 1
>   }
> }
> '
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "ind-2"
}
[elasticsearch@3e58a67ff06c elasticsearch]$ curl -X PUT "localhost:9200/ind-3?pretty" -H 'Content-Type: application/json' -d'
> {
>   "settings": {
>     "number_of_shards": 4,
>     "number_of_replicas": 2
>   }
> }
> '
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "ind-3"
}
[elasticsearch@3e58a67ff06c elasticsearch]$ 
```
Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.

![image](https://user-images.githubusercontent.com/22905019/160665046-e6011220-79c8-4717-b830-31e4a88e2571.png)

Получите состояние кластера `elasticsearch`, используя API.

![image](https://user-images.githubusercontent.com/22905019/160666055-b09f5729-c4a1-44b5-be8b-d474454a8ac3.png)

Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

- В случае индексов, в данном случае, желтый цвет обозначает нехватку места для развертывания реплик на нодах, тк. в кластете он только один. Соответсвенно для кластера это так же означает, что основные шарды распределены, а реплики нет. При этом данные могут быть недоступны до момента восстановления работосособности сбойного нода.

Удалите все индексы.

![image](https://user-images.githubusercontent.com/22905019/160667661-5c90e86a-cad4-47f2-8f98-4c48fb749f1b.png)

## Задача 3
Создайте директорию `{путь до корневой директории с elasticsearch в образе}/snapshots`.

```
user@user-pc:~/Elasticsearch$ docker exec -it es6 /bin/bash
[elasticsearch@3e58a67ff06c elasticsearch]$ sudo mkdir /usr/share/elasticsearch/snapshots
```

Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.

**Приведите в ответе** запрос API и результат вызова API для создания репозитория.

![image](https://user-images.githubusercontent.com/22905019/163004774-1e0f7440-4125-4ce3-af29-56f0802b5778.png)

Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.

![image](https://user-images.githubusercontent.com/22905019/163006180-eba02b00-299e-4966-ac81-461e4a0f13a5.png)

![image](https://user-images.githubusercontent.com/22905019/163006402-216d5c44-41fc-4752-bc18-327c0431a1c5.png)

[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.

**Приведите в ответе** список файлов в директории со `snapshot`ами.

![image](https://user-images.githubusercontent.com/22905019/163007446-7dbfa824-dce3-4cfd-9571-44b6062c1838.png)

Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.

![image](https://user-images.githubusercontent.com/22905019/163008257-2f259b03-04b8-4af7-a9b3-e1017ca03f30.png)

[Восстановите] состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 

**Приведите в ответе** запрос к API восстановления и итоговый список индексов.

![image](https://user-images.githubusercontent.com/22905019/163009121-53f59358-28da-45bc-97de-7480900e0a85.png)

