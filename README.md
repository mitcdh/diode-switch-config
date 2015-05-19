# Building a Data Diode - Switch Configs
![](network.png?raw=true "Data Diode Network Diagram")
The data diode was constructed as shown above between two Cisco switches. The following read in conjunction with the provided configuration files shows a snapshot of their status at the point we were able to demonstrate successful unidirectional networking. 

## Highside Switch

````
Highside-Switch#sh int status

Port      Name               Status       Vlan       Duplex  Speed Type
Gi3/16    Carrier-Sig        notconnect   1            full   1000 1000BaseSX
Gi3/18    Diode-Highside     connected    routed       full   1000 1000BaseSX

Highside-Switch#sh ip arp vrf Diode
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.0.0.2                -   0013.7f60.cb7f  ARPA
Internet  10.0.2.1                -   0013.80fc.dc3f  ARPA   Vlan3000
Internet  10.0.0.1                -   0013.80fc.dc3f  ARPA   GigabitEthernet3/18

Highside-Switch#sh ip route vrf Diode

Routing Table: Diode
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C       10.0.2.0/24 is directly connected, Vlan3000
C       10.0.0.0/30 is directly connected, GigabitEthernet3/18
S       10.0.1.0/24 [1/0] via 10.0.0.2
````

##Lowside Switch

````
Lowside-Switch#sh int status

Port      Name               Status       Vlan       Duplex  Speed Type
Gi2/6     Diode-Lowside      connected    routed       full   1000 1000BaseSX

Lowside-Switch#sh ip arp vrf Diode
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.0.0.2                -   0013.7f60.cb7f  ARPA   GigabitEthernet2/6
Internet  10.0.1.1                -   0013.7f60.cb7f  ARPA   Vlan3000

Lowside-Switch#sh ip route vrf Diode

Routing Table: Diode
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/30 is subnetted, 1 subnets
C       10.0.0.0 is directly connected, GigabitEthernet2/6
````
