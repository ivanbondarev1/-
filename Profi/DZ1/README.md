# **"Lab - Configure Router-on-a-Stick Inter-VLAN Routing"**
## **Топология** 
![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/Топология.png?raw=true)

## **Цели:**
+ ### Part 1: Build the Network and Configure Basic Device Settings
+ ### Part 2: Create VLANs and Assign Switch Ports
+ ### Part 3: Configure an 802.1Q Trunk between the Switches
+ ### Part 4: Configure Inter-VLAN Routing on the Router
+ ### Part 5: Verify Inter-VLAN Routing is working

## **Решение**
## **Part 1: Build the Network and Configure Basic Device Settings**

### **Step 1: Cable the network as shown in the topology.**
### **Step 2: Configure basic settings for the router.**

### R1:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/Роутер(стандарт).png?raw=true)


### **Step 3: Configure basic settings for each switch.**

### **S1**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/S1(стандарт).png?raw=true)


### **S2**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/S2(стандарт).png?raw=true)


## **Part 2: Create VLANs and Assign Switch Ports**


### **Step 1: Create VLANs on both switches.**


a.	Create and name the required VLANs on each switch from the table above.


b.	Configure the management interface and default gateway on each switch using the IP address information in the Addressing Table. 


c.	Assign all unused ports on both switches to the ParkingLot VLAN, configure them for static access mode, and administratively deactivate them.




### **Step 2: Assign VLANs to the correct switch interfaces.**


a.	Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for static access mode. Be sure to do this on both switches


b.	Issue the show vlan brief command and verify that the VLANs are assigned to the correct interfaces.


### **S1**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/S1(VLAN).png?raw=true)


### **S2**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/S2(Vlan).png?raw=true)




## **Part 3: Configure an 802.1Q Trunk Between the Switches**


### **Step 1: Manually configure trunk interface F0/1.**


a.	Change the switchport mode on interface F0/1 to force trunking. Make sure to do this on both switches.


b.	As a part of the trunk configuration, set the native VLAN to 8 on both switches. You may see error messages temporarily while the two interfaces are configured for different native VLANs.


c.	As another part of trunk configuration, specify that VLANs 3, 4, and 8 are only allowed to cross the trunk.


d.	Issue the show interfaces trunk command to verify trunking ports, the Native VLAN and allowed VLANs across the trunk.


### **Step 2: Manually configure S1’s trunk interface F0/5**


a.	Configure the F0/5 on S1 with the same trunk parameters as F0/1. This is the trunk to the router.


b.	Save the running configuration to the startup configuration file on S1 and S2.


c.	Issue the show interfaces trunk command to verify trunking.


### **S1**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/S1(tr).png?raw=true)


### **S2**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/S2(tr).png?raw=true)





## **Part 4: Configure Inter-VLAN Routing on the Router**


a.	Activate interface G0/0/1 on the router.


b.	Configure sub-interfaces for each VLAN as specified in the IP addressing table. All sub-interfaces use 802.1Q encapsulation. Ensure the sub-interface for the native VLAN does not have an IP address assigned. Include a description for each sub-interface.


c.	Use the show ip interface brief command to verify the sub-interfaces are operational.


### **R1**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/RoT.png?raw=true)


## **Part 5: Verify Inter-VLAN Routing is Working**

### **Step 1: Complete the following tests from PC-A. All should be successful.**



a.	Ping from PC-A to its default gateway.


b.	Ping from PC-A to PC-B


c.	Ping from PC-A to S2


### **PC-A**:

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/PC-A%20Test.png?raw=true)



### **Step 2: Complete the following test from PC-B.**

![](https://github.com/ivanbondarev1/Otus/blob/main/Profi/DZ1/Tracert.png?raw=true)


























































