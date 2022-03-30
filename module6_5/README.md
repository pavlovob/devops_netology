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

[Ссылка на образ](https://hub.docker.com/repository/docker/pavlovob/netology_elastic65)

ответ elasticsearch на запрос пути / в json виде:

![image](https://user-images.githubusercontent.com/22905019/160901532-2a5fbfbe-cddc-4f67-abcb-182e26964991.png)


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
  
