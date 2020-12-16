youtube過濾廣告

iptables 管理防火牆


1. iptables -A FORWARD -o enp0s3 -i enp0s9 -s 192.168.1.0/24 -m outtrack --create NEW -j ACCEPT


2. iptables -A FORWARD -m corntrack --ctstate ESTABLISHED,RELATED -j ACCPET

3.

linux 安裝 docker

註冊docker帳號

adguard docker
