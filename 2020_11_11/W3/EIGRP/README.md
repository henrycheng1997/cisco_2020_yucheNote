# eigrp router
## 設定Router ip
- en
- conf t
- hostname R1
- int e0/0
- ip addr 192.168.12.1 255.255.255.0
- no shut
## 三台設定好之後ping R2測試
- do ping 192.168.12.2
# 設定路由 eigrp
## 進入eigrp設定
- router eigrp 90
- 90為可設定的數值
- network 192.168.12.0 0.0.0.255

## eigrp的三種表
- 鄰居表(neighbor)
  - do show ip eigrp nei
- 拓樸表
  - do show ip eigrp topology
- 路由表
  - do show ip route eigrp
    -  192.168.23.0/24 [90/307200] via 192.168.12.2, 00:10:31, Ethernet0/0
    - ip [AD值/管理距離]....