commands "cairo_sw2"

(1) trunk mode between (Cairo_Sw1 & Cairo_Sw2)
Cairo_Sw2>ena
Cairo_Sw2#config t
Cairo_Sw2(config)#int g0/2
Cairo_Sw2(config-if)#switchport mode trunk

(2) create VLANs (Cairo_Sw2)
airo_Sw2(config)#vlan 20
Cairo_Sw2(config-vlan)#name pc
Cairo_Sw2(config-vlan)#vlan 30
Cairo_Sw2(config-vlan)#name lt
Cairo_Sw2(config-vlan)#vlan 50
Cairo_Sw2(config-vlan)#name pr
Cairo_Sw2(config-vlan)#int f0/1
Cairo_Sw2(config-if)#switchport access vlan 20
Cairo_Sw2(config-if)#int f0/2
Cairo_Sw2(config-if)#switchport access vlan 30
Cairo_Sw2(config-if)#int f0/3
Cairo_Sw2(config-if)#switchport access vlan 50

(3) IP for each VLAN
Cairo_Sw2(config)#int vlan 20
Cairo_Sw2(config-if)#ip address 10.10.20.2 255.255.255.0
Cairo_Sw2(config-if)#no shutdown
Cairo_Sw2(config-if)#line vty 0 5
Cairo_Sw2(config-line)#password 2024
Cairo_Sw2(config-line)#login
Cairo_Sw2(config-line)#enable password cisco

(4) default router
Cairo_Sw2(config)#ip default-gateway 10.10.20.1
*******************************************************
commands "cairo_sw1"

(1)trunk mode between (Cairo_Sw1 & Cairo_Sw2)
Cairo_Sw2>ena 
Cairo_Sw2#config t 
Cairo_Sw2(config)#int g0/2
Cairo_Sw2(config-if)#switchport mode trunk

(2)
Cairo_Sw2(config)#vlan 20
Cairo_Sw2(config-vlan)#name pc
Cairo_Sw2(config-vlan)#vlan 30
Cairo_Sw2(config-vlan)#name lt
Cairo_Sw2(config-vlan)#vlan 50
Cairo_Sw2(config-vlan)#name pr
Cairo_Sw2(config-vlan)#exit
Cairo_Sw2(config)#int f0/1
Cairo_Sw2(config-if)#switchport access vlan 20
Cairo_Sw2(config-if)#int f0/2
Cairo_Sw2(config-if)#switchport access vlan 30

(3)
Cairo_Sw2(config)#int vlan 20
Cairo_Sw2(config-if)#ip address 10.10.20.3 255.255.255.0
Cairo_Sw2(config)#line vty 0 5
Cairo_Sw2(config-line)#password 2024
Cairo_Sw2(config-line)#login
Cairo_Sw2(config-line)#enable password cisco

(4) default router
Cairo_Sw2(config-if)#ip default-gateway 10.10.20.1
Cairo_Sw2(config)#do wr
*******************************************************
commands "Cairo_R"

(1) 
Cairo_R>ena
Cairo_R#config t
Cairo_R(config)#line vty 0 5
Cairo_R(config-line)#password 2024
Cairo_R(config-line)#login
Cairo_R(config-line)#ena password cisco

(2)
Cairo_R(config)#int g0/0/0
Cairo_R(config-if)#ip address 10.0.0.1 255.255.255.252
Cairo_R(config-if)#no shutdown

(3)
cairo_R(config-if)#int g0/0/1.20
Cairo_R(config-subif)#encapsulation dot1Q 20
Cairo_R(config-subif)#ip address 10.10.20.1 255.255.255.0
Cairo_R(config-subif)#int g0/0/1.30
Cairo_R(config-subif)#encapsulation dot1Q 30
Cairo_R(config-subif)#ip address 10.10.30.1 255.255.255.0
Cairo_R(config-subif)#int g0/0/1.50
Cairo_R(config-subif)#encapsulation dot1Q 50
Cairo_R(config-subif)#ip address 10.10.50.1 255.255.255.240

(4)
Cairo_R(config)#int g0/0/1
Cairo_R(config-if)#no shutdown 

(5)
Cairo_R(config)#ip route 
Cairo_R(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
************************************************************
commands "Aswan_R"
(1)
Aswan_R>ena
Aswan_R#config t
Aswan_R(config)#line vty 0 5
Aswan_R(config-line)#password 2024
Aswan_R(config-line)#login
Aswan_R(config-line)#ena password cisco

(2)
Aswan_R(config)#int g0/0/0
Aswan_R(config-if)#ip address 10.0.0.2 255.255.255.252
Aswan_R(config-if)#no shutdown

(3)
Aswan_R(config)#int g0/0/1
Aswan_R(config-if)#ip address 10.1.1.1 255.255.255.0
Aswan_R(config-if)#no shutdown
Aswan_R(config-if)#exit
Aswan_R(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1

(4)
Aswan_R(config)#ip dhcp excluded-address 10.10.20.1 10.10.20.10
Aswan_R(config)#ip dhcp excluded-address 10.10.30.1 10.10.30.10
Aswan_R(config)#ip dhcp excluded-address 10.10.50.1 10.10.50.10	
Aswan_R(config)#ip dhcp excluded-address 10.1.1.1 10.1.1.10

Aswan_R(config)#ip dhcp pool cairo_pc
Aswan_R(dhcp-config)#network 10.10.20.0 255.255.255.0
Aswan_R(dhcp-config)#default-router 10.10.20.1 

Aswan_R(dhcp-config)#ip dhcp pool cairo_lt
Aswan_R(dhcp-config)#network 10.10.30.0 255.255.255.0
Aswan_R(dhcp-config)#default-router 10.10.30.1 

Aswan_R(dhcp-config)#ip dhcp pool cairo_pr
Aswan_R(dhcp-config)#network 10.10.50.0 255.255.255.240
Aswan_R(dhcp-config)#default-router 10.10.50.1 

Aswan_R(dhcp-config)#ip dhcp pool Aswan
Aswan_R(dhcp-config)#network 10.1.1.0 255.255.255.0
***************************************************************
commands "cairo_R"
(1)
Cairo_R>ena
Password: 
Cairo_R#config t
Cairo_R(config)interface g0/0/1.20
Cairo_R(config-subif)#ip helper-address 10.0.0.2
Cairo_R(config-subif)#interface g0/0/1.30
Cairo_R(config-subif)#ip helper-address 10.0.0.2
Cairo_R(config-subif)#interface g0/0/1.50
Cairo_R(config-subif)#ip helper-address 10.0.0.2

***************************************************************
commands "Aswan_Sw1"

Aswan_Sw1>ena
Aswan_Sw1#config t
Aswan_Sw1(config-if-range)#ip channel-group 1 mode active
Aswan_Sw1(config-if-range)#channel-group 1 mode active
Aswan_Sw1(config-if-range)#exit 
Aswan_Sw1(config)#int port-channel 1
Aswan_Sw1(config-if)#switchport mode trunk
***************************************************************
commands "Aswan_Sw2"

Switch>ena
Switch#config t
Switch(config)#int range f0/21-24
Switch(config-if-range)#channel-group 1 mode active 
Switch(config-if-range)#exit 
Switch(config)#interface port-channel 1
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
Switch(config)#do wr
**************************************************************



