# Lab-STP  
## •	Создание сети и настройка основных параметров устройства  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/Lab-STP-schema.jpg "Текст заголовка логотипа 1")  
### •	Настройте базовые параметры каждого коммутатора    
  
interface Vlan1  
 ip address 192.168.1.2 255.255.255.0  
!  
S2  
interface Vlan1  
 ip address 192.168.1.1 255.255.255.0  
!  
S3  
interface Vlan1  
 ip address 192.168.1.3 255.255.255.0  
!  
  
S3(config)#banner motd 1  
Enter TEXT message.  End with the character '1'.  
Anauthorized use is restricted  
1  
hostname S3  
!  
boot-start-marker  
boot-end-marker  
!  
!  
enable password 7 110A15040401  
!  
line con 0  
 logging synchronous  
line aux 0  
line vty 0 4  
 password 7 070C285F4D06  
 logging synchronous  
 login  
line vty 5 15  
 password 7 070C285F4D06  
 logging synchronous  
 login  
### •	Проверьте связь.  
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?	  
S1#ping 192.168.1.2  
Type escape sequence to abort.  
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:  
!!!!!  
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms  
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?	  
S1#ping 192.168.1.3  
Type escape sequence to abort.  
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:  
!!!!!  
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/6/8 ms  
Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?	  
S2#ping 192.168.1.3  
Type escape sequence to abort.  
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:  
.!!!!  
Success rate is 80 percent (4/5), round-trip min/avg/max = 3/3/4 ms  
## •	Определение корневого моста  
### •	Отключите все порты на коммутаторах.  
S1(config)#interface range gigabitEthernet 0/0-3  
S1(config-if-range)#shutdown  
S1(config)#interface r gigabitEthernet 1/0-3  
S1(config-if-range)#shutdown  
S2(config)#interface range gigabitEthernet 0/0-3  
S2(config-if-range)#shutdown  
S2(config)#interface r gigabitEthernet 1/0-3  
S2(config-if-range)#shutdown  
S3(config)#interface range gigabitEthernet 0/0-3  
S3(config-if-range)#shutdown  
S3(config)#interface r gigabitEthernet 1/0-3  
S3(config-if-range)#shutdown  

![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/all-ports.png "Текст заголовка логотипа 1")  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/all-ports-2.png "Текст заголовка логотипа 1")  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/all-ports-3.png "Текст заголовка логотипа 1")  
### •	Настройте подключенные порты в качестве транковых.  
interface GigabitEthernet0/1  
 switchport trunk allowed vlan 1  
 switchport trunk encapsulation dot1q  
 switchport mode trunk  
 shutdown  
 media-type rj45  
 negotiation auto  
!  
interface GigabitEthernet0/2  
 switchport trunk allowed vlan 1  
 switchport trunk encapsulation dot1q  
 switchport mode trunk  
 shutdown  
 media-type rj45  
 negotiation auto  
!  
interface GigabitEthernet0/3  
 switchport trunk allowed vlan 1  
 switchport trunk encapsulation dot1q  
 switchport mode trunk  
 shutdown   
 media-type rj45  
 negotiation auto  
!  
interface GigabitEthernet1/0  
 switchport trunk allowed vlan 1  
 switchport trunk encapsulation dot1q  
 switchport mode trunk  
 shutdown  
 media-type rj45  
 negotiation auto  
### •	Включите порты Gi0/2 и Gi1/0 на всех коммутаторах.  
S1(config)#interface gigabitEthernet 0/2  
S1(config)#no shutdown  
S1(config)#interface gig abitEthernet 1/0  
S1(config)#no shutdown  
S2(config)#interface gigabitEthernet 0/2  
S2(config)#no shutdown  
S2(config)#interface gigabitEthernet 1/0  
S2(config)#no shutdown  
S3(config)#interface gigabitEthernet 0/2  
S3(config)#no shutdown  
S3(config)#interface gigabitEthernet 1/0  
S3(config)#no shutdown  

![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/ports-1.png "Текст заголовка логотипа 1")  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/ports-2.png "Текст заголовка логотипа 1")  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/ports-3.png "Текст заголовка логотипа 1")  
### •	Отобразите данные протокола spanning-tree.  
S1  
VLAN0001   
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        4  
             Port        3 (GigabitEthernet0/2)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0002.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/2               Root FWD 4         128.3    Shr  
Gi1/0               Desg LRN 4         128.5    Shr  
   
S2  
VLAN0001  
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             This bridge is the root  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0001.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/2               Desg FWD 4         128.3    Shr  
Gi1/0               Desg FWD 4         128.5    Shr  
  
S3  
VLAN0001  
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        4  
             Port        3 (GigabitEthernet0/2)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0003.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/2               Root FWD 4         128.3    Shr  
Gi1/0               Altn BLK 4         128.5    Shr  
  
### Какой коммутатор является корневым мостом?     
S2  
### Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?  
Самый низкий MAC-address  
### Какие порты на коммутаторе являются корневыми портами?   
S1:Gi0/2 S2:Gi0/2  
### Какие порты на коммутаторе являются назначенными портами?   
S1:Gi1/0 S2:Gi1/0,Gi1/0  
### Какой порт отображается в качестве альтернативного и в настоящее время заблокирован?   
S3:Gi1/0  
### Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?  
Порт с более низкой стоимостью пути является предпочтительным  
### •	Измените стоимость порта.  
S3(config)# interface Gi0/2  
S3(config-if)# spanning-tree cost 18  
### •	Просмотрите изменения протокола spanning-tree.  
VLAN0001  
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        4  
             Port        3 (GigabitEthernet0/2)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0002.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
 
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/2               Root FWD 4         128.3    Shr  
Gi1/0               Desg FWD 4         128.5    Shr  
  
S3  
VLAN0001  
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        8  
             Port        5 (GigabitEthernet1/0)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0003.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/2               Altn BLK 18        128.3    Shr  
Gi1/0               Root FWD 4         128.5    Shr  
  
### Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе?  
Стоимость порта Gi0/2 уменьшилась   
### •	Удалите изменения стоимости порта.  
S3(config)# interface Gi0/2  
S3(config-if)# no spanning-tree cost 18  
### •	Повторно выполните команду show spanning-tree  
S3  
  
VLAN0001 
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        4  
             Port        3 (GigabitEthernet0/2)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0003.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/2               Root FWD 4         128.3    Shr  
Gi1/0               Altn BLK 4         128.5    Shr  
  
## •	Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов  
### •	Включите порты Gi0/1 и Gi0/3 на всех коммутаторах  
S1(config)#interface gigabitEthernet 0/1 
S1(config)#no shutdown  
S1(config)#interface gigabitEthernet 0/3  
S1(config)#no shutdown  
S2(config)#interface gigabitEthernet 0/1  
S2(config)#no shutdown  
S2(config)#interface gigabitEthernet 0/3  
S2(config)#no shutdown  
S3(config)#interface gigabitEthernet 0/1  
S3(config)#no shutdown  
S3(config)#interface gigabitEthernet 0/3  
S3(config)#no shutdown  


![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/ports-1-1.png "Текст заголовка логотипа 1")  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/ports-1-2.png "Текст заголовка логотипа 1")  
![alt-текст](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/ports-1-3.png "Текст заголовка логотипа 1") 
### •	 выполните команду show spanning-tree  
S1  
  
VLAN0001  
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        4  
             Port        2 (GigabitEthernet0/1)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
    
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0002.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/1               Root FWD 4         128.2    Shr  
Gi0/2               Altn BLK 4         128.3    Shr  
Gi0/3               Desg FWD 4         128.4    Shr  
Gi1/0               Desg LRN 4      
  
S3  
  
VLAN0001  
  Spanning tree enabled protocol rstp  
  Root ID    Priority    32769  
             Address     5000.0001.0000  
             Cost        4  
             Port        2 (GigabitEthernet0/1)  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
  
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)  
             Address     5000.0003.0000  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time  300 sec  
  
Interface           Role Sts Cost      Prio.Nbr Type  
------------------- ---- --- --------- -------- --------------------------------  
Gi0/1               Root FWD 4         128.2    Shr  
Gi0/2               Altn BLK 4         128.3    Shr  
Gi0/3               Altn BLK 4         128.4    Shr  
Gi1/0               Altn BLK 4         128.5    Shr  
  
### Какой порт выбран протоколом STP в качестве порта корневого моста на каждом коммутаторе некорневого моста?  
 Gi0/1  
### Почему протокол STP выбрал эти порты в качестве портов корневого моста на этих коммутаторах?  
Наименьший cost  
## •	Вопросы для повторения  
### •	Какое значение протокол STP использует первым после выбора корневого моста, чтобы определить выбор порта?  
Desg  
### •	Если первое значение на двух портах одинаково, какое следующее значение будет использовать протокол STP при выборе порта?  
Altn  
### •	Если оба значения на двух портах равны, каким будет следующее значение, которое использует протокол STP при выборе порта?  
Altn  
  


[R1-Config](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/Lab-STP-S1-Config.txt "")  
[S1-Config](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/Lab-STP-S2-Config.txt  "")  
[S2-Config](https://github.com/DanilChery/CiscoProjectsSTP/blob/main/Lab-STP-S3-Config.txt  "")  

