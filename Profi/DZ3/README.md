# **Lab - "Implement DHCPv4"**
## **Topology** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ3/Топология.png?raw=true)

## **Objectives:**
+ ### Part 1: Build the Network and Configure Basic Device Settings
+ ### Part 2: Configure and verify two DHCPv4 Servers on R1
+ ### Part 3: Configure and verify a DHCP Relay on R2



## **Решение**
## **Part 1: Build the Network and Configure Basic Device Settings**

### **Step 1: Establish an addressing scheme**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ3/1.png?raw=true)

### **Step 2: Cable the network as shown in the topology.**

### **Step 3: Configure basic settings for each router..**

### R1:

```host R1
no ip domain-lookup
enable secret class
line con 0
password cisco
login
exit
line vty 0 4
password cisco
login
exit
ser pass
banner motd *STAY_OUT*
end
copy run start
clock set 13:23:23 april 20 2023
```

### R2:

```host R2
no ip domain-lookup
enable secret class
line con 0
password cisco
login
exit
line vty 0 4
password cisco
login
exit
ser pass
banner motd *STAY_OUT*
end
copy run start
clock set 13:23:23 april 20 2023
```
### **Step 4: Configure Inter-VLAN Routing on R1**

```
R1(config)#int g0/0/0
R1(config-if)#ip addr 10.0.0.1 255.255.255.252
R1(config-if)#int g0/0/1.100
R1(config-subif)#enc do 100
R1(config-subif)#ip addr 192.168.1.1 255.255.255.192
R1(config-subif)#int g0/0/1.200
R1(config-subif)#enc do 200
R1(config-subif)#ip addr 192.168.1.65 255.255.255.224
R1(config-subif)#int g0/0/1.1000
R1(config-subif)#enc do 1000 native
R1(config-subif)#int ra g0/0/0-1
R1(config-if-range)#no sh
```

### **Step 5: Configure G0/0/1 on R2, then G0/0/0 and static routing for both routers**

```
R2(config)#int g0/0/1
R2(config-if)#ip addr 192.168.1.97 255.255.255.240
R2(config-if)#no sh

R2(config-if)#int g0/0/0
R2(config-if)#ip addr 10.0.0.2 255.255.255.252
R2(config-if)#no sh

R2(config-if)#exit
R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

```
R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

### **Step 6: Configure basic settings for each switch.**


### S1:

```
Switch(config)#h S1
S1(config)#no ip domain-lookup
S1(config)#enable sec class
S1(config)#line con 0
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service pas
S1(config)#logging synchronous 
S1(config)#banner motd *STAY_OUT*
S1(config)#end
S1#copy run start
```

### S2:

```
Switch(config)#h S2
S2(config)#no ip domain-lookup
S2(config)#enable sec class
S2(config)#line con 0
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#line vty 0 4
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#service pas
S2(config)#logging synchronous 
S2(config)#banner motd *STAY_OUT*
S2(config)#end
S2#copy run start
```


### **Step 7: Create VLANs on S1.**

### S1:

```
S1(config)#vlan 100
S1(config-vlan)#name Clients
S1(config-vlan)#vlan 200
S1(config-vlan)#name Management
S1(config-vlan)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Native
S1(config-vlan)#exit
S1(config)#int vlan 200
S1(config-if)#ip addr 192.168.1.66 255.255.255.224
S1(config-if)#exit
S1(config)#ip default-gateway 192.168.1.65
S1(config)#int fa0/6
S1(config-if)#sw ac vlan 100
S1(config-if)#int ra fa0/1-4, fa0/7-24, g0/1-2
S1(config-if-range)#sw m ac
S1(config-if-range)#sw ac vlan 999
S1(config-if-range)#sh
%LINK-5-CHANGED: Interface Vlan200, changed state to up
```

### S2:

```
S2(config)#int vlan 1
S2(config-if)#ip addr 192.168.1.98 255.255.255.240
S2(config-if)#exit
S2(config)#ip default-gateway 192.168.1.97
S2(config)#int ra fa0/1-4, fa0/6-7, fa0/19-24, g0/1-2
S2(config-if-range)#sw m ac
S2(config-if-range)#sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down
```

### **Step 8: Assign VLANs to the correct switch interfaces.**

### S1:

```
S1(config-if)#int ra fa0/1-4, fa0/7-24, g0/1-2
S1(config-if-range)#sw m ac 
S1(config-if-range)#sw ac vlan 999
S1(config-if-range)#exit
S1(config)#int fa0/6
S1(config-if)#sw ac vlan 100
S1(config-if)#int vlan 200
S1(config-if)#ip addr 192.168.1.66 255.255.255.224
S1(config-if)#int ra fa0/1-4, fa0/7-24, g0/1-2
S1(config-if-range)#sw m ac 
S1(config-if-range)#sw ac vlan 999
S1(config-if-range)#sh
```

### S2:

```
S2(config)#int vlan 1
S2(config-if)#ip addr 192.168.1.98 255.255.255.240
S2(config-if)#int fa0/18
S2(config-if)#sw m ac
```
***Question:*** Why is interface F0/5 listed under VLAN 1?
Потому что все интерфейсы по умолчанию находятся в VLAN 1

### **Step 9: Manually configure S1’s interface F0/5 as an 802.1Q trunk.**

```
S1(config)#int f0/5
S1(config-if)#sw m tr

S1(config-if)#sw tr native vlan 1000
S1(config-if)#sw tr all vlan 100,200,1000
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/5, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/5, changed state to up
```

## **Part 2: Configure and verify two DHCPv4 Servers on R1**
### **Step 1: Configure R1 with DHCPv4 pools for the two supported subnets. Only the DHCP Pool for subnet A is given below**
### **Step 2: Save your configuration**

### Vlan 100:

```
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5
R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101
R1(config)#ip dhcp pool POOL_100
R1(dhcp-config)#net 192.168.1.0 255.255.255.192
R1(dhcp-config)#do CCNA-lab.com
R1(dhcp-config)#def 192.168.1.1
R1(dhcp-config)#end
R1#copy run start
%SYS-5-CONFIG_I: Configured from console by console

Destination filename [startup-config]? 
Building configuration...
[OK]
```

### Vlan 200:

```
R1(config)#ip dhcp pool R2_Client_LAN
R1(dhcp-config)#net 192.168.1.96 255.255.255.240
R1(dhcp-config)#do CCNA-lab.com
R1(dhcp-config)#def 192.168.1.97
R1(dhcp-config)#end
R1#copy run start
%SYS-5-CONFIG_I: Configured from console by console

Destination filename [startup-config]? 
Building configuration...
```

### **Step 3: Verify the DHCPv4 Server configuration**

```
R1#show ip dhcp pool

Pool R2_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 30
 Leased addresses               : 0
 Excluded addresses             : 0
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.1.65         192.168.1.65     - 192.168.1.94      0    / 0     / 30

Pool POOL_100 :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 62
 Leased addresses               : 0
 Excluded addresses             : 0
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.1.1          192.168.1.1      - 192.168.1.62      0    / 0     / 62
 ```


 ```
 R1#show ip dhcp binding
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.1.2      0000.0C08.8D18           --                     Automatic
```

### **Step 3: Verify the DHCPv4 Server configuration**
### **Step 4: Attempt to acquire an IP address from DHCP on PC-A**
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: CCNA-lab.com
   Link-local IPv6 Address.........: FE80::200:CFF:FE08:8D18
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.1.2
   Subnet Mask.....................: 255.255.255.192
   Default Gateway.................: ::
                                     192.168.1.1

Bluetooth Connection:

   Connection-specific DNS Suffix..: CCNA-lab.com
   Link-local IPv6 Address.........: ::
   IPv6 Address....................: ::
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: ::
                                     0.0.0.0

C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
    
```

## **Part 3: Configure and verify a DHCP Relay on R2**

```
R2(config)#int g0/0/1 
R2(config-if)#ip help 10.0.0.1
R2(config-if)#end
R2#copy run start
%SYS-5-CONFIG_I: Configured from console by console

Destination filename [startup-config]? 
Building configuration...
[OK]
```



### **Step 2: Attempt to acquire an IP address from DHCP on PC-B**

```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: CCNA-lab.com
   Link-local IPv6 Address.........: FE80::201:97FF:FE60:58EA
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.1.99
   Subnet Mask.....................: 255.255.255.240
   Default Gateway.................: ::
                                     192.168.1.97

Bluetooth Connection:

   Connection-specific DNS Suffix..: CCNA-lab.com
   Link-local IPv6 Address.........: ::
   IPv6 Address....................: ::
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: ::
                                     0.0.0.0
```


```
C:\>ping 192.168.1.65

Pinging 192.168.1.65 with 32 bytes of data:

Reply from 192.168.1.65: bytes=32 time<1ms TTL=254
Reply from 192.168.1.65: bytes=32 time<1ms TTL=254
Reply from 192.168.1.65: bytes=32 time<1ms TTL=254
Reply from 192.168.1.65: bytes=32 time=9ms TTL=254

Ping statistics for 192.168.1.65:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 9ms, Average = 2ms
```





# **Lab - "Configure DHCPv6"**
## **Topology** 

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ3/Топология.png?raw=true)



## **Objectives:**
+ ### Part 1: Build the Network and Configure Basic Device Settings
+ ### Part 2: Verify SLAAC address assignment from R1
+ ### Part 3: Configure and verify a Stateless DHCPv6 Server on R1
+ ### Part 4: Configure and verify a Stateful DHCPv6 Server on R1
+ ### Part 5: Configure and verify a DHCPv6 Relay on R2



## **Part 1: Build the Network and Configure Basic Device Settings**


### **Step 1: Cable the network as shown in the topology.**

### **Step 2: Configure basic settings for each switch. (Optional)**


### S1:
```
Switch(config)#h S1
S1(config)#no ip domain-lookup
S1(config)#enable sec class
S1(config)#line con 0
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service pas
S1(config)#logging synchronous 
S1(config)#banner motd *STAY_OUT*
S1(config)#end
S1#copy run start
```

### S2:

```
Switch(config)#h S2
S2(config)#no ip domain-lookup
S2(config)#enable sec class
S2(config)#line con 0
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#line vty 0 4
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#service pas
S2(config)#logging synchronous 
S2(config)#banner motd *STAY_OUT*
S2(config)#end
S2#copy run start
```


### **Step 3: Configure basic settings for each router.**


### R1:

```
Router(config)#host R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#ser pass
R1(config)#banner motd *STAY_OUT*
R1(config)#ipv6 unicast-routing 
R1(config)#end
R1#copy run start
```

### R2:

```
Router(config)#host R2
R2(config)#no ip domain-lookup
R2(config)#enable secret class
R2(config)#line con 0
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#ser pass
R2(config)#banner motd *STAY_OUT*
R2(config)#end
R2#copy run start
```


### **Step 4: Configure interfaces and routing for both routers.**


### R1:

```
R1(config)#int g0/0/0
R1(config-if)#ipv6 addr 2001:db8:acad:2::1/64
R1(config-if)#ipv6 addr fe80::1 link
R1(config-if)#no sh
R1(config-if)#int g0/0/1
R1(config-if)#ipv6 addr 2001:db8:acad:1::1/64
R1(config-if)#ipv6 addr fe80::1 link
R1(config-if)#no sh
R2(config-if)#exit
R1(config)#ipv6 route ::/0 2001:db8:acad:2::2
```

### R2:

```
R2(config-if)#int g0/0/0
R2(config-if)#ipv6 addr 2001:db8:acad:2::2/64
R2(config-if)#ipv6 addr fe80::2 link
R2(config-if)#no sh
R2(config-if)#int g0/0/1
R2(config-if)#ipv6 addr 2001:db8:acad:3::1/64
R2(config-if)#ipv6 addr fe80::1 link
R2(config-if)#no sh
R2(config-if)#exit
R2(config)#ipv6 route ::/0 2001:db8:acad:2::1
```


## **Part 2: Verify SLAAC Address Assignment from R1**

### PC-A:

```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::202:17FF:FEDB:7E4
   IPv6 Address....................: 2001:DB8:ACAD:1:202:17FF:FEDB:7E4
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
```

***Вопрос:*** Откуда взялась часть адреса с идентификатором хоста?  Ответ: сработал механизм EUI-64, который позволил хосту в IPv6 самостоятельно генерировать себе идентификатор интерфейса


## **Part 3: Configure and Verify a DHCPv6 server on R1**

### **Step 1: Examine the configuration of PC-A in more detail.**

### PC-A:

```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0002.17DB.07E4
   Link-local IPv6 Address.........: FE80::202:17FF:FEDB:7E4
   IPv6 Address....................: 2001:DB8:ACAD:1:202:17FF:FEDB:7E4
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-26-E9-35-74-00-02-17-DB-07-E4
   DNS Servers.....................: ::
                                     0.0.0.0
```

### **Step 2: Configure R1 to provide stateless DHCPv6 for PC-A.**


### R1:

```
R1(config)#ipv6 dhcp pool R1-STATELESS
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATELESS.com
R1(config-dhcpv6)#interface g0/0/1
R1(config-if)#ipv6 nd other-config-flag
R1(config-if)#ipv6 dhcp server R1-STATELESS
R1(config-if)#end
R1#copy run start
```

### PC-A:

```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: STATELESS.com 
   Physical Address................: 0002.17DB.07E4
   Link-local IPv6 Address.........: FE80::202:17FF:FEDB:7E4
   IPv6 Address....................: 2001:DB8:ACAD:1:202:17FF:FEDB:7E4
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 293254888
   DHCPv6 Client DUID..............: 00-01-00-01-26-E9-35-74-00-02-17-DB-07-E4
   DNS Servers.....................: 2001:DB8:ACAD::254
                                     0.0.0.0
```


## **Part 4: Configure a stateful DHCPv6 server on R1**


### R1:

```
R1(config)#ipv6 dhcp pool R2-STATEFUL
R1(config-dhcpv6)#address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATEFUL.com
R1(config-dhcpv6)#int g0/0/0
R1(config-if)#ipv6 dhcp server R2-STATEFUL
```

## **Part 5: Configure and verify DHCPv6 relay on R2.**

### **Step 1: Power on PC-B and examine the SLAAC address that it generates.**

![](https://raw.githubusercontent.com/ivanbondarev1/Otus/main/DZ8/PC-B(ipconfig).png)
































































































