# Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"
# Задание 1
Мой внешний IP и подключение к серверу:  
![image](https://user-images.githubusercontent.com/22905019/145704314-a1f449d7-d7dc-422c-b451-520c2703ff15.png)
Маршруты к хосту: 
show ip route:  
~~~
route-views>show ip route 128.75.142.89                
Routing entry for 128.75.140.0/22
  Known via "bgp 6447", distance 20, metric 0
  Tag 6939, type external
  Last update from 64.71.137.241 2d21h ago
  Routing Descriptor Blocks:
  * 64.71.137.241, from 64.71.137.241, 2d21h ago
      Route metric is 0, traffic share count is 1
      AS Hops 3
      Route tag 6939
      MPLS label: none
route-views>
~~~
show bgp:  
~~~
route-views>show bgp 128.75.142.89     
BGP routing table entry for 128.75.140.0/22, version 1400707418
Paths: (23 available, best #9, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3549 3356 3216 3216 3216 8402, (aggregated by 64514 78.107.138.36)
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4476 3356:2 3356:22 3356:100 3356:123 3356:503 3356:903 3356:2067 3549:2581 3549:30840 8402:1023
      path 7FE10B4D89C8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 1103 3216 8402, (aggregated by 64514 78.107.138.36)
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin incomplete, localpref 100, valid, external
      Community: 3216:2001 3216:4476 8402:1023 65000:52254
      path 7FE147AD8870 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  101 174 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
      Origin incomplete, localpref 100, valid, external
      Community: 101:20100 101:20110 101:22100 174:21000 174:22013
      Extended Community: RT:101:22100
      path 7FE05D59DD28 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 3257 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE003CC1EE8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 3216 3216 3216 8402, (aggregated by 64514 78.107.138.36)
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4476 3356:2 3356:22 3356:100 3356:123 3356:503 3356:903 3356:2067 8402:1023
      path 7FE095DCF5C8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  53767 14315 6453 6762 3216 8402, (aggregated by 64514 78.107.138.36)
    162.251.163.2 from 162.251.163.2 (162.251.162.3)
      Origin IGP, localpref 100, valid, external
      Community: 14315:5000 53767:5000
      path 7FE046EBBAF0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 3491 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 1273:12276 3216:2001 3216:4476 3491:3000 3491:3022 3491:9002 8402:1023 57866:100 57866:109
      path 7FE0A2988720 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 3216 8402, (aggregated by 64514 78.107.138.36)
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE11C0B1E98 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 3216 8402, (aggregated by 64514 78.107.138.36)
    64.71.137.241 from 64.71.137.241 (216.218.252.164)
      Origin IGP, localpref 100, valid, external, best
      path 7FE184005608 RPKI State not found
      rx pathid: 0, tx pathid: 0x0
  Refresh Epoch 1
  7660 2516 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    203.181.248.168 from 203.181.248.168 (203.181.248.168)
      Origin incomplete, localpref 100, valid, external
      Community: 2516:1030 7660:9003
      path 7FE0DA353310 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  2497 3216 8402, (aggregated by 64514 78.107.138.36)
    202.232.0.2 from 202.232.0.2 (58.138.96.254)
      Origin incomplete, localpref 100, valid, external
      path 7FE02FC6CC38 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin incomplete, localpref 100, valid, external
      Community: 3257:8070 3257:30352 3257:50001 3257:53900 3257:53902 20912:65004
      path 7FE04B37F548 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 6762 3216 8402, (aggregated by 64514 78.107.138.36)
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin incomplete, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE0BCC9B5C8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 3
  3303 3216 8402, (aggregated by 64514 78.107.138.36)
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin incomplete, localpref 100, valid, external
      Community: 3216:2001 3216:4476 3303:1004 3303:1006 3303:1030 3303:3056 8402:1023
      path 7FE0C97452B0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  4901 6079 3257 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin incomplete, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE0F6F77028 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  49788 12552 3216 8402, (aggregated by 64514 78.107.138.36)
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin incomplete, localpref 100, valid, external
      Community: 12552:12000 12552:12600 12552:12601 12552:22000
      Extended Community: 0x43:100:1
      path 7FE088D80DA8 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  8283 3216 8402, (aggregated by 64514 78.107.138.36)
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin incomplete, metric 0, localpref 100, valid, external
      Community: 3216:2001 3216:4476 8283:1 8283:101 8402:1023 65000:52254
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x18
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 
      path 7FE0383776C0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1221 4637 3216 8402, (aggregated by 64514 78.107.138.36)
    203.62.252.83 from 203.62.252.83 (203.62.252.83)
      Origin incomplete, localpref 100, valid, external
      path 7FE0D3DC8058 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  701 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    137.39.3.55 from 137.39.3.55 (137.39.3.55)
      Origin incomplete, localpref 100, valid, external
      path 7FE0E2542B60 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3257 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    89.149.178.10 from 89.149.178.10 (213.200.83.26)
      Origin incomplete, metric 10, localpref 100, valid, external
      Community: 3257:8052 3257:30244 3257:50001 3257:54900 3257:54901
      path 7FE14F385C80 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 6939 3216 8402, (aggregated by 64514 78.107.138.36)
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE107968448 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  19214 174 1273 3216 8402, (aggregated by 64514 78.107.138.36)
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
      Origin incomplete, localpref 100, valid, external
      Community: 174:21000 174:22013
      path 7FE15F6F23E0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 3216 3216 3216 8402, (aggregated by 64514 78.107.138.36)
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE0EBD0BB48 RPKI State not found
      rx pathid: 0, tx pathid: 0
route-views>
~~~
traceroute:  
~~~
route-views>traceroute 128.75.142.89   
Type escape sequence to abort.
Tracing the route to 128-75-142-89.broadband.corbina.ru (128.75.142.89)
VRF info: (vrf in name/id, vrf out name/id)
  1 vl-51-gw.uoregon.edu (128.223.51.1) [AS 3582] 1 msec 0 msec 1 msec
  2 vl-51-gw.uoregon.edu (128.223.51.1) [AS 3582] 0 msec 0 msec 0 msec
  3 10.252.20.5 [AS 174] 1 msec
    10.252.19.1 [AS 174] 15 msec 18 msec
  4 10.252.9.249 [AS 174] 1 msec 0 msec 1 msec
  5 10.252.9.250 [AS 174] 0 msec 1 msec
    10.252.10.246 [AS 174] 0 msec
  6 eugn-pe1-gw.nero.net (207.98.68.190) [AS 3701] 1 msec 1 msec
    eugn-pe2-gw.nero.net (207.98.68.226) [AS 3701] 0 msec
  7 eugn-p2-gw.nero.net (207.98.64.204) [AS 3701] [MPLS: Label 300496 Exp 0] 1 msec
    eugn-p2-gw.nero.net (207.98.64.77) [AS 3701] [MPLS: Label 300496 Exp 0] 1 msec
    eugn-p2-gw.nero.net (207.98.64.204) [AS 3701] [MPLS: Label 300496 Exp 0] 1 msec
  8 ptck-p2-gw.nero.net (207.98.64.211) [AS 3701] [MPLS: Label 301232 Exp 0] 5 msec 5 msec 6 msec
  9 ptck-pe2-gw.nero.net (207.98.64.84) [AS 3701] 4 msec 4 msec 3 msec
 10 te0-4-0-33.ccr21.pdx01.atlas.cogentco.com (38.142.108.49) [AS 174] 4 msec 3 msec 3 msec
 11 be3693.ccr21.sfo01.atlas.cogentco.com (154.54.81.65) [AS 174] 20 msec 19 msec 19 msec
 12 be3670.ccr41.sjc03.atlas.cogentco.com (154.54.43.14) [AS 174] 20 msec
    be3669.ccr41.sjc03.atlas.cogentco.com (154.54.43.10) [AS 174] 20 msec 20 msec
 13 vodafone.sjc03.atlas.cogentco.com (154.54.11.206) [AS 174] 20 msec 20 msec 20 msec
 14 ae16-xcr1.chg.cw.net (195.2.24.98) [AS 1273] [MPLS: Label 553193 Exp 2] 163 msec 163 msec 163 msec
 15 195.2.31.6 [AS 1273] [MPLS: Label 157365 Exp 2] 163 msec 163 msec 163 msec
 16 et-10-1-5-xcr1.ptl.cw.net (195.2.24.242) [AS 1273] [MPLS: Label 331318 Exp 2] 154 msec 155 msec 175 msec
 17 ae1-pcr1.fnt.cw.net (195.2.9.125) [AS 1273] 164 msec 164 msec 164 msec
 18 217.161.65.146 [AS 1273] 203 msec 202 msec 202 msec
 19  *  *  * 
 20 79.104.62.86 [AS 3216] 227 msec 205 msec
    79.104.58.234 [AS 3216] 209 msec
 21  *  *  * 
 22  *  *  * 
 23  *  *  * 
 24  *  *  * 
 25  *  *  * 
 26  *  *  * 
 27  *  *  * 
 28  *  *  * 
 29  *  *  * 
 30  *  *  * 
route-views>

~~~

# Задание 2
# Задание 3
# Задание 4
# Задание 5

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
```
telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32
```
2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали. 

 ---
## Задание для самостоятельной отработки (необязательно к выполнению)

6*. Установите Nginx, настройте в режиме балансировщика TCP или UDP.

7*. Установите bird2, настройте динамический протокол маршрутизации RIP.

8*. Установите Netbox, создайте несколько IP префиксов, используя curl проверьте работу API.
