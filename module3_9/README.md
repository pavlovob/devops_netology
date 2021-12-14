# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"
# Задание 1
Установлен Bitwarden плагин для браузера Google Chrome. Пароли сохранены:  
![image](https://user-images.githubusercontent.com/22905019/145946982-97e1b74d-578a-4743-b6cd-daf1c4aea43f.png)  
# Задание 2
Google authenticator на мобильный телефон установлен, вход в Bitwarden через ОТР настроен:  
![image](https://user-images.githubusercontent.com/22905019/145948023-de3012a8-6dca-4527-8596-68889b91915a.png)  
# Задание 3
Сервер Apache2 установлен:  
![image](https://user-images.githubusercontent.com/22905019/145948826-2affa2f3-8c7f-499d-a0eb-4efe33cf0211.png)  
Сертификат сгенерирован:  
![image](https://user-images.githubusercontent.com/22905019/145950393-318b883e-7c74-40ae-ac2f-679f42ba463e.png)  
Сервер настроен на использование TLS:  
![image](https://user-images.githubusercontent.com/22905019/145950538-029f562d-7a71-4fe3-995b-de2a5ed297d7.png)  
![image](https://user-images.githubusercontent.com/22905019/145950136-9310b900-7015-4efa-9873-d362f6770467.png)  
# Задание 4
Скрипт загружен:  
![image](https://user-images.githubusercontent.com/22905019/145952109-2bf641a3-e3be-4d16-a58b-cb37676e68a8.png)  
Результат выполнения проверки (потенциальная уязвимость в github.com LUCKY13:  
![image](https://user-images.githubusercontent.com/22905019/145952268-3e8bb33e-7e86-444c-a05b-df936e81f906.png)  
# Задание 5
ssh ключ был сгенерирован ранее (dev-машина, менять не стал):  
![image](https://user-images.githubusercontent.com/22905019/145952668-15e69951-0bd5-4fe8-b28d-d62860c5a72f.png)  
Загрузка на V-box ubuntu и  вход по ssh без пароля (записан принудительно -f):  
![image](https://user-images.githubusercontent.com/22905019/145952964-f252561e-9628-47a2-a6b7-df35fd6211fb.png)  
# Задание 6
Конфигурационный файл (самый простой):  
![image](https://user-images.githubusercontent.com/22905019/145958270-9114c27c-f6da-4d23-83b3-a3443f6a2d6e.png)  
Переименование файла и вход на сервер:  
![image](https://user-images.githubusercontent.com/22905019/145958399-acf93470-49cc-471c-a118-8bd4784d2261.png)  
# Задание 7
Список интерфейсов:  
![image](https://user-images.githubusercontent.com/22905019/145960151-9e342d3d-d941-409f-b78b-54aad65928cd.png)  
Запуск tcpdump (100 пакетов) на интерфейсе, который смотрит в интернет:  
![image](https://user-images.githubusercontent.com/22905019/145962889-f3278c54-39da-4b9a-a383-5d5a05594f69.png)   
Открытие полученного файла в wireshark:  
![image](https://user-images.githubusercontent.com/22905019/145962755-61011a80-31b3-4e2d-9ca4-056127b78ca8.png)  
## Задание для самостоятельной отработки (необязательно к выполнению)
Выполнение сканирования scanme.nmap.org:  
![image](https://user-images.githubusercontent.com/22905019/145963464-2dbfa3b8-97e6-4753-8a5e-b156f2296d38.png)  
Запущены: ssh сервер, web сервер,+ пара портов непонятных сервисов )
Настройка фаервола ufw:  
![image](https://user-images.githubusercontent.com/22905019/145964132-23b176eb-2d0c-49a4-99d8-422b1a81dbe7.png)  
т.к. машина рабочая, не донастроил правила по умолчанию `sudo ufw default deny incoming` и `sudo ufw default deny outgoing` добавил указанные порты в обе стороны.  
