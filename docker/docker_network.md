# Docker Network Overview
 > network 전반에 대한 정리를 이곳에 하겠다. 
 
## docker daemon iptables 설정
-  docker daemon를 실행하면 아래와 iptables 설정을 하게 된다. 
-  각각이 왜 필요한지 알고가자.
```
....
``` 
## docker bridge network 구조 (default 설정)
- docker daemon를 실행하면 host에 docker0 virtual network interface가 생성이 된다. 
```
host # ifconfig
docker0   Link encap:Ethernet  HWaddr 02:42:16:AB:23:A1
          inet addr:172.17.0.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:16ff:feab:23a1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:17657624405 errors:0 dropped:0 overruns:0 frame:0
          TX packets:17872436172 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:2062934987440 (1.8 TiB)  TX bytes:4390147708919 (3.9 TiB)
```
- IP는 자동으로 172.17.0.1(내부 IP), netmask 255.255.0.0 설정 
- docker0는 container가 통신하기 위한 가상의 linux bridge (리눅스 bridge 개념 이해)
- bridge는 L2기반 통신
- 외부와 통신이 필요한 container, 즉 --publish(-p) 옵션으로 실행되는 컨테이너는 아래와 같은 과정으로 네트워크가 구성이 된다. 
- container가 생성될 때마다 container vritual interface가 하나씩 docker0 bridge에 binding된다.
- container는 외부와 통신하기 위해서 무조건 docker0 인터페이스를 지나야 한다. (docker0가 gateway)

- container 실행마다 생성되는 virtual network interface
```
host # ifconfig
vethb33a1a0 Link encap:Ethernet  HWaddr 0E:44:4C:FC:52:9E
          inet6 addr: fe80::c44:4cff:fefc:529e/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3188665497 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2810598800 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:402893442476 (375.2 GiB)  TX bytes:1213335520819 (1.1 TiB)

vethcbe1619 Link encap:Ethernet  HWaddr 5A:8F:9B:E1:BD:51
          inet6 addr: fe80::588f:9bff:fee1:bd51/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:732213703 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1023158669 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:88508249471 (82.4 GiB)  TX bytes:288080148069 (268.2 GiB)

veth1fe1ae2 Link encap:Ethernet  HWaddr 4E:50:C5:6B:AA:EB
          inet6 addr: fe80::4c50:c5ff:fe6b:aaeb/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2826843118 errors:0 dropped:0 overruns:0 frame:0
          TX packets:3154796174 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:511878743352 (476.7 GiB)  TX bytes:1364528884183 (1.2 TiB)

veth5da6899 Link encap:Ethernet  HWaddr 32:68:FF:1F:51:B3
          inet6 addr: fe80::3068:ffff:fe1f:51b3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9998091362 errors:0 dropped:0 overruns:0 frame:0
          TX packets:9997555239 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1196253731377 (1.0 TiB)  TX bytes:1348116407740 (1.2 TiB)

 ```
 - ip link 상태를 보면 container의 virtual network interface가 docker0에 바인딩 된 것을 확인할 수 있다.
 ```
 host # ip link
 1: lo: ....
 2: bond0: ....
 3: eth0: ...
 4: eth1: ...
 5: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN
    link/ipip 0.0.0.0 brd 0.0.0.0
 6: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN
    link/tunnel6 :: brd ::
 7: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:08:20:d1:43 brd ff:ff:ff:ff:ff:ff
 27: vethd8428d0@if26: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker0 state UP
    link/ether f2:d6:a9:54:11:1c brd ff:ff:ff:ff:ff:ff
 41: vethe781d3e@if40: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker0 state UP
    link/ether e2:73:56:c7:24:5a brd ff:ff:ff:ff:ff:ff
 43: vethe462539@if42: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker0 state UP
    link/ether fe:1a:aa:48:5c:14 brd ff:ff:ff:ff:ff:ff
 49: vethcc09590@if48: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker0 state UP
    link/ether 0e:15:0d:69:21:15 brd ff:ff:ff:ff:ff:ff
 ```
 - vethXXX로 구성된 네트워크 링크들이 docker0를 master로 up된 것을 확인할 수 있다.
 - 또한 docker0 bridge 상태를 보면 vethXXX가 생성된 것을 확인 할 수 있다.
 ```
 host # brctl show docker0
 bridge name     bridge id               STP enabled     interfaces
 docker0         8000.02420820d143       no              vethd8428d0
                                                         vethe781d3e
                                                         vethe462539
                                                         vethcc09590
 ```
 - virtual network interface와 pair로 docker-proxy도 생성이 된다. 
```
host $ ps -elf | grep docker-proxy
docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 32782 -container-ip 172.17.0.6 -container-port 20734
docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 32777 -container-ip 172.17.0.2 -container-port 20770
docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 32778 -container-ip 172.17.0.4 -container-port 20708
docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 32781 -container-ip 172.17.0.5 -container-port 20734

4개의 container가 container 내부 특정 port(20734, 20770, 20708, 20734)를 외부로 publish 하도록 실행이 된 것이다. 
```
- 동일하게 iptables에 DOCKER chain에도 설정이 된다.
```
host # iptables -nL
Chain DOCKER (2 references)
target     prot opt source               destination
ACCEPT     tcp  --  0.0.0.0/0            172.17.0.2          tcp dpt:20770
ACCEPT     tcp  --  0.0.0.0/0            172.17.0.4          tcp dpt:20708
ACCEPT     tcp  --  0.0.0.0/0            172.17.0.5          tcp dpt:20734
ACCEPT     tcp  --  0.0.0.0/0            172.17.0.6          tcp dpt:20734
```
- iptables에 의해서 외부 패킷이 바로 docker0 bridge를 타고 내부 container로 흘러 들어가게 된다.
- 위에서 docker-proxy도 iptables와 동일한 역할을 하게 되는데, 정상적인 경우는 iptables를 통해서 패킷 교환이 되고, docker-proxy는 전혀 아무 동작도 하지 않는다. iptables설정에 문제가 있을 경우 docker-proxy를 통해서 통신이 이루어 지게 된다. 일종에 통신에 문제가 발생할 경우 안전 장치로 설정을 해 놓은 것이다.
- container가 외부와 통신을 하기 위해서는 총 3개의 설정이 이루어짐을 알 수 있다.
  - docker0 bridge interface에 바인딩
  - docker-proxy 프로세스 동작
  - iptables에 ACCEPT chain 추가

- vethXXX 네트워크 인터페이스는 container내부에서는 네트워크 네임스페이스가 완전히 분리되어 보통의 ethernet 인터페이스로 보인다.
```
host # docker exec a7ca7ffc2143 ifconfig eth0
eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:06
          inet addr:172.17.0.6  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe11:6/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:700868600 errors:0 dropped:0 overruns:0 frame:0
          TX packets:701281173 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:94516537032 (88.0 GiB)  TX bytes:84013167602 (78.2 GiB)
 ```
- container 내부 route 상태를 보면 docker0의 IP가 gateway로 잡혀있는 것을 확인해 볼 수 있다. 
```
# docker exec a7ca7ffc2143 route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         172.17.0.1      0.0.0.0         UG    0      0        0 eth0
172.17.0.0      *               255.255.0.0     U     0      0        0 eth0
```

## docker swarm mode network 구조 (default 설정)

