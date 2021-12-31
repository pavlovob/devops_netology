### Задание 1
Выполнено, VM установлена:  
![image](https://user-images.githubusercontent.com/22905019/147818835-944ea376-2bb1-4287-9f4c-09efb01d8c7c.png)

### Задание 2
Установка и настройка ufw:  
![image](https://user-images.githubusercontent.com/22905019/147546793-993763ed-e4f9-404c-9383-06b32bc23892.png)  
![image](https://user-images.githubusercontent.com/22905019/147547531-3258016c-fd6f-4877-b4f0-89617b09a3e2.png)  
![image](https://user-images.githubusercontent.com/22905019/147547657-0452199c-ffd2-47af-8192-5f69ca9aaa7d.png)  
### Задание 3
Установка vault (поправил дефолтное правило UFW на `allow outgoing`:  
~~~
user@sbn04piapp04:~$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
curl: (6) Could not resolve host: apt.releases.hashicorp.com
gpg: no valid OpenPGP data found.
user@sbn04piapp04:~$ ^C
user@sbn04piapp04:~$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
user@sbn04piapp04:~$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
OK
user@sbn04piapp04:~$ ^C
user@sbn04piapp04:~$ sudo ufw reload
Firewall reloaded
user@sbn04piapp04:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                  
443/tcp                    ALLOW       Anywhere                  
22/tcp (v6)                ALLOW       Anywhere (v6)             
443/tcp (v6)               ALLOW       Anywhere (v6)             

user@sbn04piapp04:~$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
OK
user@sbn04piapp04:~$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
Hit:1 http://ru.archive.ubuntu.com/ubuntu focal InRelease                                                         
Get:2 https://apt.releases.hashicorp.com focal InRelease [9495 B]                                                 
Get:3 http://ru.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:4 http://ru.archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
Get:5 https://apt.releases.hashicorp.com focal/main amd64 Packages [41.1 kB]
Get:6 http://ru.archive.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Get:7 http://ru.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [1400 kB]
Get:8 http://ru.archive.ubuntu.com/ubuntu focal-updates/main Translation-en [283 kB]
Get:9 http://ru.archive.ubuntu.com/ubuntu focal-updates/main amd64 c-n-f Metadata [14.7 kB]
Get:10 http://ru.archive.ubuntu.com/ubuntu focal-updates/restricted amd64 Packages [616 kB]
Get:11 http://ru.archive.ubuntu.com/ubuntu focal-updates/restricted Translation-en [88.1 kB]                          
Get:12 http://ru.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [884 kB]                             
Get:13 http://ru.archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [193 kB]                             
Get:14 http://ru.archive.ubuntu.com/ubuntu focal-updates/universe amd64 c-n-f Metadata [19.9 kB]                      
Get:15 http://ru.archive.ubuntu.com/ubuntu focal-backports/main amd64 Packages [42.0 kB]                              
Get:16 http://ru.archive.ubuntu.com/ubuntu focal-backports/main Translation-en [10.0 kB]                              
Get:17 http://ru.archive.ubuntu.com/ubuntu focal-backports/main amd64 c-n-f Metadata [864 B]                          
Get:18 http://ru.archive.ubuntu.com/ubuntu focal-backports/universe amd64 Packages [18.9 kB]                          
Get:19 http://ru.archive.ubuntu.com/ubuntu focal-backports/universe Translation-en [7492 B]                           
Get:20 http://ru.archive.ubuntu.com/ubuntu focal-backports/universe amd64 c-n-f Metadata [636 B]                      
Get:21 http://ru.archive.ubuntu.com/ubuntu focal-security/main amd64 Packages [1069 kB]                               
Get:22 http://ru.archive.ubuntu.com/ubuntu focal-security/main Translation-en [197 kB]                                
Get:23 http://ru.archive.ubuntu.com/ubuntu focal-security/main amd64 c-n-f Metadata [9096 B]                          
Get:24 http://ru.archive.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [566 kB]                          
Get:25 http://ru.archive.ubuntu.com/ubuntu focal-security/restricted Translation-en [80.9 kB]                         
Get:26 http://ru.archive.ubuntu.com/ubuntu focal-security/universe amd64 Packages [668 kB]                            
Get:27 http://ru.archive.ubuntu.com/ubuntu focal-security/universe Translation-en [112 kB]                            
Get:28 http://ru.archive.ubuntu.com/ubuntu focal-security/universe amd64 c-n-f Metadata [13.0 kB]                     
Fetched 6679 kB in 9s (749 kB/s)                                                                                      
Reading package lists... Done
user@sbn04piapp04:~$ sudo apt-get update && sudo apt-get install vault
Hit:1 http://ru.archive.ubuntu.com/ubuntu focal InRelease       
Hit:2 http://ru.archive.ubuntu.com/ubuntu focal-updates InRelease
Hit:3 http://ru.archive.ubuntu.com/ubuntu focal-backports InRelease
Hit:4 http://ru.archive.ubuntu.com/ubuntu focal-security InRelease                
Hit:5 https://apt.releases.hashicorp.com focal InRelease                          
Reading package lists... Done
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  vault
0 upgraded, 1 newly installed, 0 to remove and 15 not upgraded.
Need to get 69.4 MB of archives.
After this operation, 188 MB of additional disk space will be used.
Get:1 https://apt.releases.hashicorp.com focal/main amd64 vault amd64 1.9.2 [69.4 MB]
Fetched 69.4 MB in 1min 39s (702 kB/s)                                                                                
Selecting previously unselected package vault.
(Reading database ... 71567 files and directories currently installed.)
Preparing to unpack .../archives/vault_1.9.2_amd64.deb ...
Unpacking vault (1.9.2) ...
Setting up vault (1.9.2) ...
Generating Vault TLS key and self-signed certificate...
Generating a RSA private key
........................................................................................................++++
.................................................................................................................................................................................................................++++
writing new private key to 'tls.key'
-----
Vault TLS key and self-signed certificate have been generated in '/opt/vault/tls'.
user@sbn04piapp04:~$ vault
Usage: vault <command> [args]

Common commands:
    read        Read data and retrieves secrets
    write       Write data, configuration, and secrets
    delete      Delete secrets and configuration
    list        List data or secrets
    login       Authenticate locally
    agent       Start a Vault agent
    server      Start a Vault server
    status      Print seal and HA status
    unwrap      Unwrap a wrapped secret

Other commands:
    audit          Interact with audit devices
    auth           Interact with auth methods
    debug          Runs the debug command
    kv             Interact with Vault's Key-Value storage
    lease          Interact with leases
    monitor        Stream log messages from a Vault server
    namespace      Interact with namespaces
    operator       Perform operator-specific tasks
    path-help      Retrieve API help for paths
    plugin         Interact with Vault plugins and catalog
    policy         Interact with policies
    print          Prints runtime configurations
    secrets        Interact with secrets engines
    ssh            Initiate an SSH session
    token          Interact with tokens
user@sbn04piapp04:~$ 

~~~

### Задание 1
### Задание 1
### Задание 1
### Задание 1
### Задание 1
### Задание 1
### Задание 1
### Задание 1

3. Установите hashicorp vault ([инструкция по ссылке](https://learn.hashicorp.com/tutorials/vault/getting-started-install?in=vault/getting-started#install-vault)).
4. Cоздайте центр сертификации по инструкции ([ссылка](https://learn.hashicorp.com/tutorials/vault/pki-engine?in=vault/secrets-management)) и выпустите сертификат для использования его в настройке веб-сервера nginx (срок жизни сертификата - месяц).
5. Установите корневой сертификат созданного центра сертификации в доверенные в хостовой системе.
6. Установите nginx.
7. По инструкции ([ссылка](https://nginx.org/en/docs/http/configuring_https_servers.html)) настройте nginx на https, используя ранее подготовленный сертификат:
  - можно использовать стандартную стартовую страницу nginx для демонстрации работы сервера;
  - можно использовать и другой html файл, сделанный вами;
8. Откройте в браузере на хосте https адрес страницы, которую обслуживает сервер nginx.
9. Создайте скрипт, который будет генерировать новый сертификат в vault:
  - генерируем новый сертификат так, чтобы не переписывать конфиг nginx;
  - перезапускаем nginx для применения нового сертификата.
10. Поместите скрипт в crontab, чтобы сертификат обновлялся какого-то числа каждого месяца в удобное для вас время.

## Результат

Результатом курсовой работы должны быть снимки экрана или текст:

- Процесс установки и настройки ufw
- Процесс установки и выпуска сертификата с помощью hashicorp vault
- Процесс установки и настройки сервера nginx
- Страница сервера nginx в браузере хоста не содержит предупреждений 
- Скрипт генерации нового сертификата работает (сертификат сервера ngnix должен быть "зеленым")
- Crontab работает (выберите число и время так, чтобы показать что crontab запускается и делает что надо)

## Как сдавать курсовую работу

Курсовую работу выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Также вы можете выполнить задание в [Google Docs](https://docs.google.com/document/u/0/?tgif=d) и отправить в личном кабинете на проверку ссылку на ваш документ.
Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Перед тем как выслать ссылку, убедитесь, что ее содержимое не является приватным (открыто на комментирование всем, у кого есть ссылка), иначе преподаватель не сможет проверить работу. 
Ссылка на инструкцию [Как предоставить доступ к файлам и папкам на Google Диске](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop).
