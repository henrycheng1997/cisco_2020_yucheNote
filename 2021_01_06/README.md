# GRE(Generic Routing Encapsulation) 通用路由封裝
-  https://www.jannet.hk/zh-Hant/post/generic-routing-encapsulation-gre/

# Hub-to-spoke Topology
## 拓樸圖
- 網路 ![](./01.jpg)
- 自製 ![](./02.jpg)
## 設定router:
- loop back
- ip
- rip(R1、R2、R3互通)
### R1
```
en
conf t
host R1
int lo 0
ip addr 192.168.1.1 255.255.255.0
no shut
int e0/0
ip addr 10.0.14.1 255.255.255.0
no shut

router rip
version 2
network 10.0.14.0
network 192.168.1.0
```
### R2
```
en
conf t
host R2
int lo 0
ip addr 192.168.2.2 255.255.255.0
no shut
int e0/0
ip addr 10.0.24.2 255.255.255.0
no shut

router rip
version 2
network 10.0.24.0
network 192.168.2.0
```
### R3
```
en
conf t
host R3
int lo 0
ip addr 192.168.3.3 255.255.255.0
no shut
int e0/0
ip addr 10.0.34.3 255.255.255.0
no shut

router rip
version 2
network 10.0.34.0
network 192.168.3.0
```
### R4
```
en
conf t
host R4
int e0/0
ip addr 10.0.14.4 255.255.255.0
no shut
int e0/1
ip addr 10.0.24.4 255.255.255.0
no shut
int e0/2
ip addr 10.0.34.4 255.255.255.0
no shut

router rip
version 2
network 10.0.14.0
network 10.0.24.0
network 10.0.34.0
```
## 設定Tunnel(完成後要再加上rip)
## R1、R2
### R1
```
int tunnel 12
ip addr 172.16.12.1 255.255.255.0
tunnel source ethernet 0/0
tunnel destination 10.0.24.2
```
### R2
```
int tunnel 12
ip addr 172.16.12.2 255.255.255.0
tunnel source ethernet 0/0
tunnel destination 10.0.14.1
```
## R1、R3
### R1
```
int tunnel 13
ip addr 172.16.13.1 255.255.255.0
tunnel source ethernet 0/0
tunnel destination 10.0.34.3
```
### R3
```
int tunnel 13
ip addr 172.16.13.3 255.255.255.0
tunnel source ethernet 0/0
tunnel destination 10.0.14.1
```
### R1、R2、R3
```
router rip
version 2
network 172.16.12.0
network 172.16.13.0
```
- 測試連線
  - R1(config)# do ping 172.16.12.2 source 172.16.12.1
  - R1(config)# do ping 172.16.13.3 source 172.16.12.1
  - R2(config)# do ping 172.16.13.3 source 172.16.12.2
- 查看路由表
  - do show ip int brief
## Fully Mesh Topology
- ![](./03.jpg)
### R2
```
int tunnel 23
ip addr 172.16.23.2 255.255.255.0
tunnel source ethernet 0/0
tunnel destination 10.0.34.3
router rip
network 172.16.23.0
```
### R3
```
int tunnel 23
ip addr 172.16.23.3 255.255.255.0
tunnel source ethernet 0/0
tunnel destination 10.0.24.2
router rip
network 172.16.23.0
```
# IPsec(Internet Protocol Security)國際網路安全協定
- https://www.jannet.hk/zh-Hant/post/internet-protocol-security-ipsec/

## 起始設定
- topo
- ![](./04.jpg)
- R1
```
en
conf t
host R1
int e0/0
ip addr 192.168.13.1 255.255.255.0
no shut
int e0/1
ip addr 192.168.10.1 255.255.255.0
no shut

ip route 0.0.0.0 0.0.0.0 192.168.13.3
```
- R2
```
en
conf t
host R2
int e0/0
ip addr 192.168.23.2 255.255.255.0
no shut
int e0/1
ip addr 192.168.20.1 255.255.255.0
no shut

ip route 0.0.0.0 0.0.0.0 192.168.23.3
```
- R3
```
en
conf t
host R3
int e0/0
ip addr 192.168.13.3 255.255.255.0
no shut
int e0/1
ip addr192.168.23.3 255.255.255.0
no shut

ip route 0.0.0.0 0.0.0.0 192.168.13.3
```
## Interesting







- 補充教材
  - https://www.jannet.hk/zh-Hant/post/gre-over-ipsec-vs-ipsec-over-gre/