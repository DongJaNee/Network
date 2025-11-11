## Rhel 환경 ethernet network bonding
### 1. 사전확인 
```
nmcli device status
```

### 2. 본딩 인터페이스 생성
```
sudo nmcli connection add type bond ifname bond0 mode active-backup
```

### 3. 실제 NIC를 본딩 슬레이브로 추가 
```
sudo nmcli connection add type ethernet ifname ens3f0 master bond0
sudo nmcli connection add type ethernet ifname ens3f1 master bond0
```

### 4. IP 주소 설정 
```
sudo nmcli connection modify bond-bond0 ipv4.addresses 192.168.10.100/24
sudo nmcli connection modify bond-bond0 ipv4.gateway 192168.10.1
sudo nmcli connection modify bond-bond0 ipv4.method manual
sudo nmcli connection up bond-bond0
```

### 5. 설정 적용 확인
```
nmcli connection up bond-slave-ens3f0
nmcli connection up bond-slave-ens3f1
```

```
cat /proc/net/bonding/bond0
```

### 6. Bonding 추가
```
sudo nmcli connection add type ethernet ifname ens3f2 master bond0       // ens3f2를 bond0의 slave로 추가 
sudo nmcli connection up bond-slave-ens3f2       // ens3f2 slave 연결 활성화 

nmcli device status      // 상태확인
cat /proc/net/bonding/bond0       // bond0에 ens3f2가 추가되었는지 확인
```

