### Задание 1
Выполнено, VM установлена:  
![image](https://user-images.githubusercontent.com/22905019/147818835-944ea376-2bb1-4287-9f4c-09efb01d8c7c.png)

### Задание 2
Установка UFW:  
~~~
user@vboxpc:~$ sudo apt install ufw
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  ufw
0 upgraded, 1 newly installed, 0 to remove and 41 not upgraded.
Need to get 147 kB of archives.
After this operation, 849 kB of additional disk space will be used.
Get:1 http://ru.archive.ubuntu.com/ubuntu focal-updates/main amd64 ufw all 0.36-6ubuntu1 [147 kB]
Fetched 147 kB in 0s (609 kB/s)
Preconfiguring packages ...
Selecting previously unselected package ufw.
(Reading database ... 177028 files and directories currently installed.)
Preparing to unpack .../ufw_0.36-6ubuntu1_all.deb ...
Unpacking ufw (0.36-6ubuntu1) ...
Setting up ufw (0.36-6ubuntu1) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for rsyslog (8.2001.0-1ubuntu1.1) ...
Processing triggers for systemd (245.4-4ubuntu3.13) ...
user@vboxpc:~$ 
~~~
настройка ufw:  
~~~
user@vboxpc:~$ sudo ufw status
Status: inactive
user@vboxpc:~$ sudo ufw enable
Firewall is active and enabled on system startup
user@vboxpc:~$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
user@vboxpc:~$ sudo ufw default deny incoming
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
user@vboxpc:~$ sudo ufw allow in 22/tcp
Rule added
Rule added (v6)
user@vboxpc:~$ sudo ufw allow in 443/tcp
Rule added
Rule added (v6)
user@vboxpc:~$ sudo ufw reload
Firewall reloaded
user@vboxpc:~$ 
user@vboxpc:~$ sudo ufw status
Status: active
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                  
443/tcp                    ALLOW       Anywhere                  
22/tcp (v6)                ALLOW       Anywhere (v6)             
443/tcp (v6)               ALLOW       Anywhere (v6)             
user@vboxpc:~$ 
~~~
### Задание 3
Установка hasicorp vault:  
~~~
user@vboxpc:~$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
OK
user@vboxpc:~$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
Hit:1 http://ru.archive.ubuntu.com/ubuntu focal InRelease
Hit:2 http://ru.archive.ubuntu.com/ubuntu focal-updates InRelease                                  
Hit:3 http://ru.archive.ubuntu.com/ubuntu focal-backports InRelease                                
Hit:4 http://packages.microsoft.com/repos/code stable InRelease                                    
Hit:5 https://dl.google.com/linux/chrome/deb stable InRelease                                      
Hit:6 http://security.ubuntu.com/ubuntu focal-security InRelease                                   
Hit:7 https://packages.microsoft.com/repos/vscode stable InRelease                                 
Get:8 https://apt.releases.hashicorp.com focal InRelease [9 495 B]
Get:9 https://apt.releases.hashicorp.com focal/main amd64 Packages [41,1 kB]
Fetched 50,6 kB in 1s (34,6 kB/s)    
Reading package lists... Done
user@vboxpc:~$ sudo apt-get update && sudo apt-get install vault
Hit:1 http://ru.archive.ubuntu.com/ubuntu focal InRelease
Hit:2 http://ru.archive.ubuntu.com/ubuntu focal-updates InRelease                                  
Hit:3 http://ru.archive.ubuntu.com/ubuntu focal-backports InRelease                                
Hit:4 https://dl.google.com/linux/chrome/deb stable InRelease                                      
Hit:5 http://packages.microsoft.com/repos/code stable InRelease                                    
Hit:6 http://security.ubuntu.com/ubuntu focal-security InRelease                                   
Hit:7 https://apt.releases.hashicorp.com focal InRelease                                           
Hit:8 https://packages.microsoft.com/repos/vscode stable InRelease              
Reading package lists... Done                             
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  vault
0 upgraded, 1 newly installed, 0 to remove and 41 not upgraded.
Need to get 69,4 MB of archives.
After this operation, 188 MB of additional disk space will be used.
Get:1 https://apt.releases.hashicorp.com focal/main amd64 vault amd64 1.9.2 [69,4 MB]
Fetched 69,4 MB in 1min 19s (878 kB/s)                                                             
Selecting previously unselected package vault.
(Reading database ... 177119 files and directories currently installed.)
Preparing to unpack .../archives/vault_1.9.2_amd64.deb ...
Unpacking vault (1.9.2) ...
Setting up vault (1.9.2) ...
Generating Vault TLS key and self-signed certificate...
Generating a RSA private key
..................++++
.....++++
writing new private key to 'tls.key'
-----
Vault TLS key and self-signed certificate have been generated in '/opt/vault/tls'.
user@vboxpc:~$ vault
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
user@vboxpc:~$ 
~~~
### Задание 4
Cоздайте центр сертификации по инструкции (ссылка) и выпустите сертификат для использования его в настройке веб-сервера nginx (срок жизни сертификата - месяц).
~~~

~~~
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
