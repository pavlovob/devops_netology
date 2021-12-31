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
Cоздание центра сертификации и выпуск сертификата для nginx (срок жизни сертификата - месяц):  
Запуск сервера:  
~~~
vault server -dev -dev-root-token-id root
~~~
Экспорт переменных:  
~~~
user@vboxpc:~$ export VAULT_ADDR=http://127.0.0.1:8200
user@vboxpc:~$ export VAULT_TOKEN=root
~~~
Выпуск корневого сертификата:  
~~~
user@vboxpc:~$ vault secrets enable pki
Success! Enabled the pki secrets engine at: pki/
user@vboxpc:~$ vault secrets tune -max-lease-ttl=87600h pki
Success! Tuned the secrets engine at: pki/
user@vboxpc:~$ vault write -field=certificate pki/root/generate/internal \
>      common_name="example.com" \
>      ttl=87600h > CA_cert.crt
user@vboxpc:~$ vault write pki/config/urls \
>      issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
>      crl_distribution_points="$VAULT_ADDR/v1/pki/crl"
Success! Data written to: pki/config/urls
user@vboxpc:~$ 
~~~
Выпуск промежуточного сертификата:  
~~~
user@vboxpc:~$ vault secrets enable -path=pki_int pki
Success! Enabled the pki secrets engine at: pki_int/
user@vboxpc:~$ vault secrets tune -max-lease-ttl=43800h pki_int
Success! Tuned the secrets engine at: pki_int/
user@vboxpc:~$ vault write -format=json pki_int/intermediate/generate/internal \
>      common_name="example.com Intermediate Authority" \
>      | jq -r '.data.csr' > pki_intermediate.csr
user@vboxpc:~$ vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr \
>      format=pem_bundle ttl="43800h" \
>      | jq -r '.data.certificate' > intermediate.cert.pem
user@vboxpc:~$ vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem
Success! Data written to: pki_int/intermediate/set-signed
user@vboxpc:~$ 
~~~
Создание роли:  
~~~
user@vboxpc:~$ vault write pki_int/roles/example-dot-com \
>      allowed_domains="example.com" \
>      allow_subdomains=true \
>      max_ttl="720h"
Success! Data written to: pki_int/roles/example-dot-com
user@vboxpc:~$ 
~~~
Запрос нового сертификата для сервера со сроком жизни месяц:  
~~~
user@vboxpc:~$ vault write pki_int/issue/example-dot-com common_name="test.example.com" ttl="720h"
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDpjCCAo6gAwIBAgIUK7eZUGiV9wGobSnm+uOybVyGpUQwDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjExMjMxMTExMjU2WhcNMjYx
MjMwMTExMzI2WjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA6UmbEeEY
4uh9sW38OtuHT8KZWMlMky1yjT3sEAfwGq3YWH7IeCNPM2BtbKsLEVyHDm125SZ9
52Xm/MIQTKNG/vMUG8nJo2OVO7eP3sQ2GO7zyHlQdPlshOF/4ml+DO/k2BqJR+uL
h0DvlCcCcPHAlBdJ0Swv8QmX8WUpR3L23yTQidk929CpkmY5/2XsRdWcynq1xGYM
vgEbHvuaoF1Mw7g6riPvkrOzlaYBE1AviBI5UZzgaW9l+a5jQIfkodXvaOAFnKHk
2RKqR2YcVTdNInxtyoNbTJnwR6X6qrB9+YwwqbHXpXfc9FUq+x7PHhiWtp1CUmYy
fFXayFzk86OoeQIDAQABo4HUMIHRMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBRJ11+vvnmFfFekNNSh9rxnE8h7ETAfBgNVHSMEGDAW
gBQ0/Wdx3zplQlPiGBVtBCxFJIsNTzA7BggrBgEFBQcBAQQvMC0wKwYIKwYBBQUH
MAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwMQYDVR0fBCowKDAm
oCSgIoYgaHR0cDovLzEyNy4wLjAuMTo4MjAwL3YxL3BraS9jcmwwDQYJKoZIhvcN
AQELBQADggEBAIeiimesosVMypdexJtRzwzpD4sDUIZ/+w0hlENU7L5mH9Vr1pau
aIDiFArUVix2l+b4dIuhh6gBdIzjtrNUCqYDgRqaxagbhu9y/CfM3dIwqALXu0CW
s4+JtDs7D/rcQDfeYP/m15VCgxKjAw3yw7rk8SwZTlungBNjHnXm9/79ral0urey
btD6d0W9BnCtJYqbYRPBr9Q6BLtWY1mCCPtKymaeKE+VoB1FGioeJ76Y7DE+H+z1
anFZ6O6kuZHsszKZ0JQ6Om31NbRlxifi5kDC/akYC2mEHVGo/deT2BK7FA4VwA7Y
3sYwNsGR/a2s0hEie405twMtyV0fFkpQKKw=
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZjCCAk6gAwIBAgIUV1mpW7A+jFhx7bdXlgsLNU2DxuMwDQYJKoZIhvcNAQEL
BQAwLTErMCkGA1UEAxMiZXhhbXBsZS5jb20gSW50ZXJtZWRpYXRlIEF1dGhvcml0
eTAeFw0yMTEyMzExMTIwMDhaFw0yMjAxMzAxMTIwMzdaMBsxGTAXBgNVBAMTEHRl
c3QuZXhhbXBsZS5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCq
MAmsjOE5HS/qElAchctl4F5zdysDi2FtDrcJMcMQt2WkZN/1VTGF6vDBzRoorzZl
qKetHN90SPMPf2kgvs1rF/zoRT3Q5D2CXbyh+upvAmeEJtcsctI2lLpYVzZTwmVk
4NCkNx26jDAbmTdoztmNibxLJhekiC8+5ZTgT+B8v1aKYsZ57vONNUkq30H4tzQH
To0RIkUNDfr1VFLU4I5/4JJFY6uHw7h099AD6qU2lyc39jLvOpgyeYBo1p1xR48w
FiGMgksBaYuklUjCZcC+P2R70Rj9C/6lKuXWOnLznSOdQJkrLnd/YqcN+FCZiX50
gko+7rt+YQxYuq0J/vDnAgMBAAGjgY8wgYwwDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUJv+V1subhZo2R1HJ
3SIaBgk5UWEwHwYDVR0jBBgwFoAUSddfr755hXxXpDTUofa8ZxPIexEwGwYDVR0R
BBQwEoIQdGVzdC5leGFtcGxlLmNvbTANBgkqhkiG9w0BAQsFAAOCAQEAkWkGOVES
3GCH8LncuL/gxVJIiOzNWv7orz4guyatqIbOWWA0U77PbhOy+KdtgmR0n0auXe50
1/zctqk3VNjAMVlCqvbSSK4bOvxJI+HDV/nc73BIWBMZixqXuVm6tdmj7ITIvmsx
ACdpLkIn4RPOYLFVNgMc/7PZnsWIpQxlshKI3/YxMmx3affh/8DHrchaJ+Y89wzc
S7MblBcAmL7kdNUbjN682flsef+JUWfN4l/jsT9TTnH0n6okZROJW9cy0LG/aW6z
WN8T9F9bMGEQeOOLkD6LwXEj9VgMehX5f2fTdwsM91VYRlPbz9QCk6mnRAHpiEuV
YvmwKD1h+dtDDw==
-----END CERTIFICATE-----
expiration          1643541637
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDpjCCAo6gAwIBAgIUK7eZUGiV9wGobSnm+uOybVyGpUQwDQYJKoZIhvcNAQEL
BQAwFjEUMBIGA1UEAxMLZXhhbXBsZS5jb20wHhcNMjExMjMxMTExMjU2WhcNMjYx
MjMwMTExMzI2WjAtMSswKQYDVQQDEyJleGFtcGxlLmNvbSBJbnRlcm1lZGlhdGUg
QXV0aG9yaXR5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA6UmbEeEY
4uh9sW38OtuHT8KZWMlMky1yjT3sEAfwGq3YWH7IeCNPM2BtbKsLEVyHDm125SZ9
52Xm/MIQTKNG/vMUG8nJo2OVO7eP3sQ2GO7zyHlQdPlshOF/4ml+DO/k2BqJR+uL
h0DvlCcCcPHAlBdJ0Swv8QmX8WUpR3L23yTQidk929CpkmY5/2XsRdWcynq1xGYM
vgEbHvuaoF1Mw7g6riPvkrOzlaYBE1AviBI5UZzgaW9l+a5jQIfkodXvaOAFnKHk
2RKqR2YcVTdNInxtyoNbTJnwR6X6qrB9+YwwqbHXpXfc9FUq+x7PHhiWtp1CUmYy
fFXayFzk86OoeQIDAQABo4HUMIHRMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBRJ11+vvnmFfFekNNSh9rxnE8h7ETAfBgNVHSMEGDAW
gBQ0/Wdx3zplQlPiGBVtBCxFJIsNTzA7BggrBgEFBQcBAQQvMC0wKwYIKwYBBQUH
MAKGH2h0dHA6Ly8xMjcuMC4wLjE6ODIwMC92MS9wa2kvY2EwMQYDVR0fBCowKDAm
oCSgIoYgaHR0cDovLzEyNy4wLjAuMTo4MjAwL3YxL3BraS9jcmwwDQYJKoZIhvcN
AQELBQADggEBAIeiimesosVMypdexJtRzwzpD4sDUIZ/+w0hlENU7L5mH9Vr1pau
aIDiFArUVix2l+b4dIuhh6gBdIzjtrNUCqYDgRqaxagbhu9y/CfM3dIwqALXu0CW
s4+JtDs7D/rcQDfeYP/m15VCgxKjAw3yw7rk8SwZTlungBNjHnXm9/79ral0urey
btD6d0W9BnCtJYqbYRPBr9Q6BLtWY1mCCPtKymaeKE+VoB1FGioeJ76Y7DE+H+z1
anFZ6O6kuZHsszKZ0JQ6Om31NbRlxifi5kDC/akYC2mEHVGo/deT2BK7FA4VwA7Y
3sYwNsGR/a2s0hEie405twMtyV0fFkpQKKw=
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAqjAJrIzhOR0v6hJQHIXLZeBec3crA4thbQ63CTHDELdlpGTf
9VUxherwwc0aKK82ZainrRzfdEjzD39pIL7Naxf86EU90OQ9gl28ofrqbwJnhCbX
LHLSNpS6WFc2U8JlZODQpDcduowwG5k3aM7ZjYm8SyYXpIgvPuWU4E/gfL9WimLG
ee7zjTVJKt9B+Lc0B06NESJFDQ369VRS1OCOf+CSRWOrh8O4dPfQA+qlNpcnN/Yy
7zqYMnmAaNadcUePMBYhjIJLAWmLpJVIwmXAvj9ke9EY/Qv+pSrl1jpy850jnUCZ
Ky53f2KnDfhQmYl+dIJKPu67fmEMWLqtCf7w5wIDAQABAoIBAFy9ergVuTHHbvHN
8uvoGPO2NaIUQVNYI8orJ6ESBetZFUFlWwm02BzS70mcu/GJsUnqgxz5y+bxTcqX
MrGOaCBA3Sexe8MWbVJaRE28jv3ZQJGqHL1zAIyPtZAoTkmMeHZlcCzcgA3FEP4p
GyG4/qJ7eSk2Y9HcCGrs7pjhzkILmo70VY3sIR2LsBy4xtsPwOc3Th81D/eILzC9
loBKCE50XuEVV4GghsEgCxeTgXDqh/UPcNB/hkS7XLMRBI4EwwXnuY+pq2oW3JVa
JR9+gSfpAVhwYPDhkTDg4mfLVicI8qMA3ST+QhnFcme4ZF4XufaI4NyFswRem9eY
DmE0vmECgYEAxhJPC8SFn8ygQLs3I06q7+RvVG2KZVilNrLSaQ0bmb0nMST55XBa
IqdwdHXGKzKgZSc+h8jmquE4dHEsKZHxeXw0OO4BODEGUdJK2KU2rXmbfk8TIe/L
qzZw2zesl5hMZdymnIKqGDBkEGSs+n8porCoA3D82NzlL4vHiYgErpECgYEA2/YN
hJnNM8UT/DnGEmEHFXFBX7Ai6lEQMpga8H91lbpA6hgs3jhUL2gimzDLC5mLTLPM
tVJpArLPP2AH0JJyf9wa+Mm4T8DLbuTJbWoF6uo/UcgPBV9ApAsm47KxDasEvuZN
jtPGTiXF6GgsD4OCt6DD4U5lYjCAHJxoNCaJ0/cCgYBdzLXaYMrXDlSl0wMdmVei
G5ANb4Km1AAJk03Jqgd0GvvAbj5ZxYcp+hlrTYr3UhZbUOZv71gtfFL78cx0M0Uj
vwoMG8pADhdsECaZykPGi1xzyIbK/4B4KGPxrL/zWpBzfLb6T3a11dTNXp/8UNQq
03X9izhyismOZqesHdn5wQKBgCJ5zmSaNq+GlDtUUtdOne2ecsCsusw/KGrFrHNF
hwiQyNvoLiAmdAt6JvJsE2ceCddb1xoUcKEbpaApTRBD4+5mcVPNSjY14azf7zJX
C9ZmIMaQtMoCw/7yQIYv29BonbXOIxnf15UoFnz21vEXi8V8TTdjMkDRmULiwPJr
l+7BAoGAJFi2D0TOFAxu7OPuSuqb3vmPIHCcvnqqpXT7KvkXvxxyATXRLlKlR16U
/2tLIKnVd2Wq440k848MaXznT0vJXgm76rNWm5i9SqvEabn7p9hDXyANAbJITqaW
UFW+8B8PpMMCO9MZj3wniTNzs9ax7xUNbfZ7tKquY0WmjAKqxVk=
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       57:59:a9:5b:b0:3e:8c:58:71:ed:b7:57:96:0b:0b:35:4d:83:c6:e3
user@vboxpc:~$ 
~~~
### Задание 5
Установка корневого сертификата в хотсовую сиситему:  
![image](https://user-images.githubusercontent.com/22905019/147821135-67c1f63f-0917-4b1e-988f-53587bedffd5.png)  
### Задание 6
Установка NGINX:  
~~~
user@vboxpc:~$ sudo apt-add-repository ppa:nginx/stable
user@vboxpc:~$ sudo apt update
user@vboxpc:~$ sudo apt install nginx
~~~

~~~
~~~
### Задание 1
### Задание 1
### Задание 1
### Задание 1
### Задание 1
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
