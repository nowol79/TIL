# Docker Network Overview
 > network 전반에 대한 정리를 이곳에 하겠다. 
 
## docker daemon iptables 설정
-  docker daemon를 실행하면 아래와 iptables 설정을 하게 된다. 
-  각각이 왜 필요한지 알고가자.
```
time="2016-08-03T13:21:05.776930955+09:00" level=debug msg="/sbin/iptables, [--wait --version]"
time="2016-08-03T13:21:05.777982679+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -D PREROUTING -m addrtype --dst-type LOCAL -j DOCKER]"
time="2016-08-03T13:21:05.779369801+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -D OUTPUT -m addrtype --dst-type LOCAL ! --dst 127.0.0.0/8 -j DOCKER]"
time="2016-08-03T13:21:05.780666794+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -D OUTPUT -m addrtype --dst-type LOCAL -j DOCKER]"
time="2016-08-03T13:21:05.781930353+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -D PREROUTING]"
time="2016-08-03T13:21:05.783047011+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -D OUTPUT]"
time="2016-08-03T13:21:05.784161519+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -F DOCKER]"
time="2016-08-03T13:21:05.785265331+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -X DOCKER]"
time="2016-08-03T13:21:05.786365025+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -F DOCKER]"
time="2016-08-03T13:21:05.787549465+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -X DOCKER]"
time="2016-08-03T13:21:05.788614818+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -F DOCKER-ISOLATION]"
time="2016-08-03T13:21:05.789783533+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -X DOCKER-ISOLATION]"
time="2016-08-03T13:21:05.790839938+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -n -L DOCKER]"
time="2016-08-03T13:21:05.791860877+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -N DOCKER]"
time="2016-08-03T13:21:05.792945398+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -n -L DOCKER]"
time="2016-08-03T13:21:05.793994743+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -n -L DOCKER-ISOLATION]"
time="2016-08-03T13:21:05.795052499+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C DOCKER-ISOLATION -j RETURN]"
time="2016-08-03T13:21:05.796162855+09:00" level=debug msg="/sbin/iptables, [--wait -I DOCKER-ISOLATION -j RETURN]"
time="2016-08-03T13:21:05.815732485+09:00" level=warning msg="Could not load necessary modules for IPSEC rules: Running modprobe xfrm_user failed with message: `WARNING: Module xfrm_user not found.`, error: exit status 1"
time="2016-08-03T13:21:05.815978746+09:00" level=info msg="Default bridge (docker0) is assigned with an IP address 172.17.0.0/16. Daemon option --bip can be used to set a preferred IP address"
time="2016-08-03T13:21:05.816047156+09:00" level=debug msg="Allocating IPv4 pools for network bridge (6635ceef590326778bce4b151a7b89c56a6a7958ef8b16525dada40a7ca52697)"
time="2016-08-03T13:21:05.816078235+09:00" level=debug msg="RequestPool(LocalDefault, 172.17.0.0/16, , map[], false)"
time="2016-08-03T13:21:05.816144375+09:00" level=debug msg="RequestAddress(LocalDefault/172.17.0.0/16, 172.17.0.1, map[RequestAddressType:com.docker.network.gateway])"
time="2016-08-03T13:21:05.816398026+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -C POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE]"
time="2016-08-03T13:21:05.817846858+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -C DOCKER -i docker0 -j RETURN]"
time="2016-08-03T13:21:05.819159202+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -I DOCKER -i docker0 -j RETURN]"
time="2016-08-03T13:21:05.820496730+09:00" level=debug msg="/sbin/iptables, [--wait -D FORWARD -i docker0 -o docker0 -j DROP]"
time="2016-08-03T13:21:05.821812742+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C FORWARD -i docker0 -o docker0 -j ACCEPT]"
time="2016-08-03T13:21:05.823089995+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C FORWARD -i docker0 ! -o docker0 -j ACCEPT]"
time="2016-08-03T13:21:05.824415897+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT]"
time="2016-08-03T13:21:05.825818929+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -C PREROUTING -m addrtype --dst-type LOCAL -j DOCKER]"
time="2016-08-03T13:21:05.827168792+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -A PREROUTING -m addrtype --dst-type LOCAL -j DOCKER]"
time="2016-08-03T13:21:05.828598752+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -C OUTPUT -m addrtype --dst-type LOCAL -j DOCKER ! --dst 127.0.0.0/8]"
time="2016-08-03T13:21:05.829952234+09:00" level=debug msg="/sbin/iptables, [--wait -t nat -A OUTPUT -m addrtype --dst-type LOCAL -j DOCKER ! --dst 127.0.0.0/8]"
time="2016-08-03T13:21:05.831362447+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C FORWARD -o docker0 -j DOCKER]"
time="2016-08-03T13:21:05.832631616+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C FORWARD -o docker0 -j DOCKER]"
time="2016-08-03T13:21:05.833940019+09:00" level=debug msg="/sbin/iptables, [--wait -t filter -C FORWARD -j DOCKER-ISOLATION]"
time="2016-08-03T13:21:05.835176491+09:00" level=debug msg="/sbin/iptables, [--wait -D FORWARD -j DOCKER-ISOLATION]"
time="2016-08-03T13:21:05.836594264+09:00" level=debug msg="/sbin/iptables, [--wait -I FORWARD -j DOCKER-ISOLATION]"
time="2016-08-03T13:21:05.839845824+09:00" level=info msg="Daemon has completed initialization"
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

## docker swarm mode network 구조 
- docker swarm 모드로 init를 하게 되면 docker_gwbridge bridge가 하나더 생성이 된다. 
```
host # docker swarm init --advertise-addr=10.XX.XX.XX
Swarm initialized: current node (ei1qsaga9xzipdel2i63o1aqz) is now a manager.

To add a worker to this swarm, run the following command:
    docker swarm join \
    --token SWMTKN-1-35701hdo5h6hh3yxe0zzminijlilvb4q2qo16exsovo29sof8t-6gs7t789fg7oxvsi6pxu1ldhs \
    10.XX.XX.XX:2377

To add a manager to this swarm, run the following command:
    docker swarm join \
    --token SWMTKN-1-35701hdo5h6hh3yxe0zzminijlilvb4q2qo16exsovo29sof8t-cm0qw9vtuou5jmn26xx2a1hmw \
    10.XX.XX.XX:2377

host # ifconfig 
docker_gwbridge Link encap:Ethernet  HWaddr 02:42:6C:84:D8:AA
          inet addr:172.19.0.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:6cff:fe84:d8aa/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:61336 errors:0 dropped:0 overruns:0 frame:0
          TX packets:61336 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:5624037 (5.3 MiB)  TX bytes:5624037 (5.3 MiB)

docker0   Link encap:Ethernet  HWaddr 02:42:16:B5:F3:3C
          inet addr:172.17.0.1  Bcast:0.0.0.0  Mask:255.255.0.0
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)
vethf3569f8 Link encap:Ethernet  HWaddr 2A:D6:F9:FC:88:81
          inet6 addr: fe80::28d6:f9ff:fefc:8881/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:648 (648.0 b)  TX bytes:648 (648.0 b)

host # brctl show docker_gwbridge
bridge name bridge id           STP enabled     interfaces
docker_gwbridge         8000.02426c84d8aa no            vethf3569f8

host # ip link
... 생략
7: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN
    link/ether 02:42:16:b5:f3:3c brd ff:ff:ff:ff:ff:ff
12: docker_gwbridge: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:6c:84:d8:aa brd ff:ff:ff:ff:ff:ff
44: vethf3569f8@if43: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue master docker_gwbridge state UP
    link/ether 2a:d6:f9:fc:88:81 brd ff:ff:ff:ff:ff:ff

host # docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
6635ceef5903        bridge              bridge              local
8539cd64be76        docker_gwbridge     bridge              local
1606773c51b0        host                host                local
3ed6x4ecjzw1        ingress             overlay             swarm
dcac01c0db44        none                null                local
```
- docker_gwbridge가 생성이 되면서 vethf3569f8@if43라는 virtual network interface가 생성이 되고, docker_gwbridge 링크되어 있다. 
- docker network ls를 통해서 보면, docker_gwbridge와 overlay타입의 network가 하나 더 생성이 되는 것을 알 수 있다.

- docker daemon으로 swarm init 요청이 오면, 아래와 같은 일들이 순차적으로 일어난다.
```
time="2016-08-03T13:27:13.647361486+09:00" level=debug msg="Calling POST /v1.24/swarm/init"
time="2016-08-03T13:27:13.647499960+09:00" level=debug msg="form data: {\"AdvertiseAddr\":\"10.XX.XX.XX\",\"ForceNewCluster\":false,\"ListenAddr\":\"0.0.0.0:2377\",\"Spec\":{\"CAConfig\":{\"NodeCertExpiry\":7.776e+15},\"Dispatcher\":{\"HeartbeatPeriod\":5e+09},\"Orchestration\":{\"TaskHistoryRetentionLimit\":5},\"Raft\":{},\"TaskDefaults\":{}}}"

```
- 기본 swarm TLS통신을 위한 root CA 생성
```
time="2016-08-03T13:27:13.846169190+09:00" level=debug msg="successfully loaded the Root CA: /naver/docker/var/lib/swarm/certificates/swarm-root-ca.crt"
time="2016-08-03T13:27:13.906194707+09:00" level=debug msg="successfully loaded the Root CA: /naver/docker/var/lib/swarm/certificates/swarm-root-ca.crt"
time="2016-08-03T13:27:13.906248425+09:00" level=debug msg="loaded local CA certificate: /naver/docker/var/lib/swarm/certificates/swarm-root-ca.crt."
time="2016-08-03T13:27:13.934335118+09:00" level=debug msg="loaded local TLS credentials: /naver/docker/var/lib/swarm/certificates/swarm-node.crt."
time="2016-08-03T13:27:13.934509069+09:00" level=debug msg="Requesting certificate for NodeID: ei1qsaga9xzipdel2i63o1aqz"
time="2016-08-03T13:27:13.967175740+09:00" level=debug msg="successfully loaded the Root CA: /naver/docker/var/lib/swarm/certificates/swarm-root-ca.crt"
```
- swarm용 unix 소켓 생성 (control.sock)
```
time="2016-08-03T13:27:13.998118294+09:00" level=info msg="Listening for connections" addr="[::]:2377" proto=tcp
time="2016-08-03T13:27:13.998252106+09:00" level=info msg="Listening for local connections" addr="/naver/docker/var/lib/swarm/control.sock" proto=unix
```
- manager 모드로 실행이 되었기 때문에 manager간 서버 이중화를 위해 RAFT 프로토콜 설정을 한다. 
- RAFT 알고리즘에 따라 리더를 선출한다.
```
time="2016-08-03T13:27:13.998648437+09:00" level=info msg="ef6d2325641b243 became follower at term 0"
time="2016-08-03T13:27:13.998746858+09:00" level=info msg="newRaft ef6d2325641b243 [peers: [], term: 0, commit: 0, applied: 0, lastindex: 0, lastterm: 0]"
time="2016-08-03T13:27:13.998771929+09:00" level=info msg="ef6d2325641b243 became follower at term 1"
time="2016-08-03T13:27:13.998833739+09:00" level=info msg="ef6d2325641b243 is starting a new election at term 1"
time="2016-08-03T13:27:13.998885336+09:00" level=info msg="ef6d2325641b243 became candidate at term 2"
time="2016-08-03T13:27:13.998903349+09:00" level=info msg="ef6d2325641b243 received vote from ef6d2325641b243 at term 2"
time="2016-08-03T13:27:13.998927813+09:00" level=info msg="ef6d2325641b243 became leader at term 2"
time="2016-08-03T13:27:13.998948228+09:00" level=info msg="raft.node: ef6d2325641b243 elected leader ef6d2325641b243 at term 2"
time="2016-08-03T13:27:13.999306444+09:00" level=debug msg="ef6d2325641b243 ignoring MsgHup because already leader"
time="2016-08-03T13:27:14.000426214+09:00" level=debug msg="(*Agent).run" module=agent
time="2016-08-03T13:27:14.000702172+09:00" level=debug msg="(*session).start" module=agent
time="2016-08-03T13:27:14.000810008+09:00" level=warning msg="Could not get operating system name: Error opening /usr/lib/os-release: open /usr/lib/os-release: no such file or directory"
```
- 앞서 overlay로 생성된 swarm용 ingress 네트워크 ID(3ed6x4ecjzw1)를 join 시킨다. 
```
time="2016-08-03T13:27:14.040329340+09:00" level=debug msg="Root CA updated successfully" cluster.id=8kvzp424wmst6zgx8cxn2pfzy method="(*Server).updateCluster" module=ca
time="2016-08-03T13:27:14.042257322+09:00" level=debug msg="RequestPool(GlobalDefault, 10.255.0.0/16, , map[], false)"
time="2016-08-03T13:27:14.042335510+09:00" level=debug msg="RequestAddress(GlobalDefault/10.255.0.0/16, <nil>, map[])"
time="2016-08-03T13:27:14.043403111+09:00" level=debug msg="RequestAddress(GlobalDefault/10.255.0.0/16, <nil>, map[])"
time="2016-08-03T13:27:14.043970949+09:00" level=debug msg="RequestAddress(GlobalDefault/10.255.0.0/16, <nil>, map[])"
time="2016-08-03T13:27:14.095593412+09:00" level=debug msg="Root CA updated successfully" cluster.id=8kvzp424wmst6zgx8cxn2pfzy method="(*Server).updateCluster" module=ca
time="2016-08-03T13:27:14.225531106+09:00" level=debug msg="(*session).listen" module=agent session.id=4kndqr0ua5q12pno1ujwl0q7b
time="2016-08-03T13:27:14.225592886+09:00" level=debug msg="(*session).watch" module=agent session.id=4kndqr0ua5q12pno1ujwl0q7b
time="2016-08-03T13:27:14.225579565+09:00" level=debug msg="(*session).heartbeat" module=agent session.id=4kndqr0ua5q12pno1ujwl0q7b
time="2016-08-03T13:27:14.225951332+09:00" level=debug method="(*Dispatcher).Tasks" node.id=ei1qsaga9xzipdel2i63o1aqz node.session=4kndqr0ua5q12pno1ujwl0q7b
time="2016-08-03T13:27:14.226507972+09:00" level=info msg="Initializing Libnetwork Agent Local-addr=10.XX.XX.XX
Adv-addr=10.114.132.148 Remote-addr ="
time="2016-08-03T13:27:14.226549877+09:00" level=debug msg="agent: registered" module=agent
time="2016-08-03T13:27:14.226591995+09:00" level=debug msg="(*worker).Assign" len(tasks)=0 module=agent
time="2016-08-03T13:27:14.226622488+09:00" level=info msg="Initializing Libnetwork Agent Local-addr=10.XX.XX.XX Adv-addr=10.114.132.148 Remote-addr ="
time="2016-08-03T13:27:14.226738169+09:00" level=debug msg="Encryption key 1: d5029"
time="2016-08-03T13:27:14.226760549+09:00" level=debug msg="Encryption key 2: 70c22"
time="2016-08-03T13:27:14.226777016+09:00" level=debug msg="Encryption key 3: 0735e"
time="2016-08-03T13:27:14.227235537+09:00" level=debug msg="Initial encryption keys: [(key: 2c6f3, tag: 0x8adb) (key: e9e10, tag: 0x8ada) (key: 31e05, tag: 0x8adc)]"
time="2016-08-03T13:27:14.227416640+09:00" level=debug msg="Allocating IPv4 pools for network ingress (3ed6x4ecjzw1iig1kfbk8a4wi)"
time="2016-08-03T13:27:14.227475972+09:00" level=debug msg="RequestPool(LocalDefault, 10.255.0.0/16, , map[], false)"
time="2016-08-03T13:27:14.227528890+09:00" level=debug msg="RequestAddress(LocalDefault/10.255.0.0/16, 10.255.0.1, map[RequestAddressType:com.docker.network.gateway])"
time="2016-08-03T13:27:14.227570335+09:00" level=debug msg="overlay: Received vxlan IDs: 256"
time="2016-08-03T13:27:14.227613222+09:00" level=debug msg="/sbin/iptables, [--wait -t mangle -C OUTPUT -p udp --dport 4789 -m u32 --u32 0>>22&0x3C@12&0xFFFFFF00=65536 -j MARK --set-mark 13681891]"
time="2016-08-03T13:27:14.230096387+09:00" level=debug msg="Calling GET /v1.24/swarm"
time="2016-08-03T13:27:14.230899901+09:00" level=debug msg="css0760: joined network 3ed6x4ecjzw1iig1kfbk8a4wi"
time="2016-08-03T13:27:14.230936362+09:00" level=debug msg="css0760: Initiating bulk sync with nodes [css0760]"
```
- swarm init가 되면 docker info에 swarm 정보가 ```Swarm: inactive``` 에서 새롭게 갱신이 된다.
```
host $ docker info
Swarm: active
 NodeID: ei1qsaga9xzipdel2i63o1aqz
 Is Manager: true
 ClusterID: 8kvzp424wmst6zgx8cxn2pfzy
 Managers: 1
 Nodes: 1
 Orchestration:
  Task History Retention Limit: 5
 Raft:
  Snapshot interval: 10000
  Heartbeat tick: 1
  Election tick: 3
 Dispatcher:
  Heartbeat period: 5 seconds
 CA configuration:
  Expiry duration: 3 months
 Node Address: 10.XX.XX.XX
```
