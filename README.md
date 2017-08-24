# Building a Data Diode - Switch Configs

![](img/network-diagram.png?raw=true "Data Diode Network Diagram")

The data diode was constructed as shown above between two Cisco switches by [mitcdh](https://github.com/mitcdh), [alancowie](https://github.com/alancowie), and networking friends. The switches were chosen as an easy media converter for connecting multiple UTP on each side to test and develop routed unidirectional networking tools but with a similar fibre interface setup the configuration should be portable to other platforms.

### Configuration

* [Highside Switch](conf/high.conf)
* [Lowside Switch](conf/low.conf)

### Interfaces

![](img/network-cabling.jpg?raw=true "Data Diode Physical Cabling")

#### Highside Switch

* `TenGigabitEthernet1/1/1`: Interface not configured but open to generate a carrier signal, TX connected to Te1/1/4 (RX) also on Highside Switch.
* `TenGigabitEthernet1/1/4`: RX connected to Te1/1/1 (TX) also on Highside switch, TX connected to Te1/1/1 (RX) on Lowside Switch.

#### Lowside Switch

* `TenGigabitEthernet1/1/4`: RX connected to Te1/1/4 (TX) on Highside switch, TX not connected. Mac address configured as static ARPA record on Highside switch.

### Status Output

#### Highside Switch

````
high>sh int status

Port      Name               Status       Vlan       Duplex  Speed Type
Te1/1/1                      notconnect   1            full   1000 1000BaseSX SFP
Te1/1/4                      connected    routed       full   1000 1000BaseSX SFP

high>sh ip arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.0.2.1                -   706b.b95a.42d7  ARPA   Vlan3000
Internet  10.3.0.1                -   706b.b95a.42f4  ARPA   TenGigabitEthernet1/1/4
Internet  10.3.0.2                -   cc5a.53f9.7e74  ARPA

high>sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
S        10.0.1.0/24 [1/0] via 10.3.0.2
C        10.3.0.0/30 is directly connected, TenGigabitEthernet1/1/4
L        10.3.0.1/32 is directly connected, TenGigabitEthernet1/1/4
````

#### Lowside Switch

````
low>sh int status

Port      Name               Status       Vlan       Duplex  Speed Type
Te1/1/4                      connected    routed       full   1000 1000BaseSX SFP

low>sh ip arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.0.1.1                -   cc5a.53f9.7e57  ARPA   Vlan3000
Internet  10.3.0.2                -   cc5a.53f9.7e74  ARPA   TenGigabitEthernet1/1/4

low>sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       + - replicated route, % - next hop override

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 4 subnets, 3 masks
C        10.0.1.0/24 is directly connected, Vlan3000
L        10.0.1.1/32 is directly connected, Vlan3000
C        10.3.0.0/30 is directly connected, TenGigabitEthernet1/1/4
L        10.3.0.2/32 is directly connected, TenGigabitEthernet1/1/4
````
