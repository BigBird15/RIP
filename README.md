# RIP
Задание №2
Ткалич Надежда, КН-202, номер в журнале: 23
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1.Добавление портов
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Добавляем к роутерам дополнительные порты (используя модуль NM-1FE-TX). 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2.Настройка IP, масок и шлюзов
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  Server 0:    | IP              = 192.168.1.8
               | Subnet Mask     = 255.255.255.0

  Server 1:    | IP              = 192.168.2.17
               | Subnet Mask     = 255.255.255.0

  Server 2:    | IP              = 192.168.3.17
               | Subnet Mask     = 255.255.255.0

  PC0:         | IP              = 10.23.1.33
               | Subnet Mask     = 255.255.255.0
               | Default Gateway = 10.23.1.2
 
  PC1:         | IP              = 10.23.1.34
               | Subnet Mask     = 255.255.255.0
               | Default Gateway = 10.23.1.2
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
3.Настрока роутеров
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
***ROUTER 0***

Router>en

Router#conf t 

Router(config)#int f1/0 

Router(config-if)#no shutdown

Router(config-if)#exit

Router(config)#int f0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.252 
Router(config-if)#exit 
Router(config)#int f0/1 
Router(config-if)#ip address 10.23.1.1 255.255.255.0
Router(config-if)#exit
Router(config)#int f1/0 
Router(config-if)#ip address 192.168.1.1 255.255.255.0 
Router(config-if)#exit Router(config)

Router(config)#exit
Router#write memory

***ROUTER 1***

Router>en                                                  
Router#conf t 
Router(config)#int f1/0 
Router(config-if)#no shutdown
Router(config-if)#exit

Router(config)#int f0/0
Router(config-if)#ip address 192.168.10.21 255.255.255.252 
Router(config-if)#exit 
Router(config)#int f0/1 
Router(config-if)#ip address 192.168.10.6 255.255.255.252
Router(config-if)#exit
Router(config)#int f1/0 
Router(config-if)#ip address 192.168.2.1 255.255.255.0 
Router(config-if)#exit Router(config)

Router(config)#exit
Router#write memory

***ROUTER 2***

Router>en                                                  
Router#conf t 
Router(config)#int f1/0 
Router(config-if)#no shutdown
Router(config-if)#exit

Router(config)#int f0/0
Router(config-if)#ip address 10.23.1.2 255.255.255.0 
Router(config-if)#exit 
Router(config)#int f0/1 
Router(config-if)#ip address 192.168.10.5 255.255.255.252
Router(config-if)#exit
Router(config)#int f1/0 
Router(config-if)#ip address 192.168.3.1 255.255.255.0 
Router(config-if)#exit Router(config)

Router(config)#exit
Router#write memory

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
4.Запуск протокола RIP, вывод информации
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
***ROUTER 1***

Router>en
Router#conf t
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)# network 192.168.10.0
Router(config-router)# network 10.23.1.0
Router(config-router)# network 192.168.1.0
************************************************************************************************************
Router>en
Router#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.1    YES manual up                    up 
FastEthernet0/1        10.23.1.1       YES manual up                    up 
FastEthernet1/0        192.168.1.1     YES manual up                    up 
Vlan1                  unassigned      YES unset 
************************************************************************************************************
Router#show ip rip database
10.23.1.0/24    auto-summary
10.23.1.0/24    directly connected, FastEthernet0/1
192.168.1.0/24    auto-summary
192.168.1.0/24    directly connected, FastEthernet1/0
192.168.10.0/30    auto-summary
192.168.10.0/30    directly connected, FastEthernet0/0
*************************************************************************************************************
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
     10.0.0.0/24 is subnetted, 1 subnets
C       10.23.1.0 is directly connected, FastEthernet0/1
C    192.168.1.0/24 is directly connected, FastEthernet1/0
     192.168.10.0/30 is subnetted, 1 subnets
C       192.168.10.0 is directly connected, FastEthernet0/0


***ROUTER 1***
Router>en
Router#conf t
Router>en
Router#conf t
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)# network 192.168.10.0
Router(config-router)# network 192.168.10.4
Router(config-router)# network 192.168.2.0
****************************************************************************************************
Router>en
Router#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.10.2    YES manual up                    up 
FastEthernet0/1        192.168.10.6    YES manual up                    up 
FastEthernet1/0        192.168.2.1     YES manual up                    up 
Vlan1                  unassigned      YES unset 
****************************************************************************************************
Router#show ip rip database
10.0.0.0/8    auto-summary
10.0.0.0/8
    [1] via 192.168.10.5, 00:00:11, FastEthernet0/1
192.168.2.0/24    auto-summary
192.168.2.0/24    directly connected, FastEthernet1/0
192.168.3.0/24    auto-summary
192.168.3.0/24
    [1] via 192.168.10.5, 00:00:11, FastEthernet0/1
192.168.10.0/30    auto-summary
192.168.10.0/30    directly connected, FastEthernet0/0
192.168.10.4/30    auto-summary
192.168.10.4/30    directly connected, FastEthernet0/1
*****************************************************************************************************
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

R    10.0.0.0/8 [120/1] via 192.168.10.5, 00:00:16, FastEthernet0/1
C    192.168.2.0/24 is directly connected, FastEthernet1/0
R    192.168.3.0/24 [120/1] via 192.168.10.5, 00:00:16, FastEthernet0/1
     192.168.10.0/30 is subnetted, 2 subnets
C       192.168.10.0 is directly connected, FastEthernet0/0
C       192.168.10.4 is directly connected, FastEthernet0

***ROUTER 2***

Router>en
Router#conf t
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)# network 10.23.1.0
Router(config-router)# network 192.168.10.4
Router(config-router)# network 192.168.3.0
***************************************************************************************************************
Router>en
Router#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        10.23.1.2       YES manual up                    up 
FastEthernet0/1        192.168.10.5    YES manual up                    up 
FastEthernet1/0        192.168.3.1     YES manual up                    up 
Vlan1                  unassigned      YES unset 
***************************************************************************************************************
Router#show ip rip database
10.23.1.0/24    auto-summary
10.23.1.0/24    directly connected, FastEthernet0/0
192.168.2.0/24    auto-summary
192.168.2.0/24
    [1] via 192.168.10.6, 00:00:09, FastEthernet0/1
192.168.3.0/24    auto-summary
192.168.3.0/24    directly connected, FastEthernet1/0
192.168.10.0/30    auto-summary
192.168.10.0/30
    [1] via 192.168.10.6, 00:00:09, FastEthernet0/1
192.168.10.4/30    auto-summary
192.168.10.4/30    directly connected, FastEthernet0/1
*****************************************************************************************************************
Router#show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is subnetted, 1 subnets
C       10.23.1.0 is directly connected, FastEthernet0/0
R    192.168.2.0/24 [120/1] via 192.168.10.6, 00:00:02, FastEthernet0/1
C    192.168.3.0/24 is directly connected, FastEthernet1/0
     192.168.10.0/30 is subnetted, 2 subnets
R       192.168.10.0 [120/1] via 192.168.10.6, 00:00:02, FastEthernet0/1
C       192.168.10.4 is directly connected, FastEthernet0/1



