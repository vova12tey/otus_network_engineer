# Конфигурации устройств (Configurations)

Этот файл содержит основные команды настройки протоколов, использованных в проекте. Полные конфигурации всех устройств находятся в папке `configs/`.

## 1. VLAN и Trunk (уровень L2)

### Коммутатор ядра S1-Core
```bash
vlan 10
 name IT
vlan 20
 name Accounting
vlan 30
 name Guests
vlan 50
 name Branch_SPb
vlan 60
 name Branch_Novosibirsk
vlan 99
 name Management
vlan 100
 name DMZ
vlan 1000
 name Native
!
interface range f0/1-6
 switchport mode trunk
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,30,50,60,99,100
Коммутатор доступа S1-User
bash
vlan 10
 name IT
vlan 20
 name Accounting
vlan 30
 name Guests
vlan 99
 name Management
!
interface f0/1
 switchport mode trunk
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,30,99
!
interface f0/6
 switchport mode access
 switchport access vlan 10
interface f0/7
 switchport mode access
 switchport access vlan 20
interface f0/8
 switchport mode access
 switchport access vlan 30
Коммутатор S1-Server
bash
vlan 99
 name Management
vlan 100
 name DMZ
!
interface f0/1
 switchport mode trunk
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 99,100
!
interface f0/6
 switchport mode access
 switchport access vlan 99
interface f0/7
 switchport mode access
 switchport access vlan 100
Пояснение: VLAN разделяют сеть на отделы (ИТ, Бухгалтерия, Гости), а также выделяют отдельные сегменты для управления (VLAN 99) и DMZ (VLAN 100). Транки (802.1Q) передают трафик всех VLAN между коммутаторами и маршрутизаторами.

2. Динамическая маршрутизация (OSPF)
R1-Main (Москва)
bash
router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.1.0 0.0.0.3 area 0
 network 10.0.2.0 0.0.0.3 area 0
 network 10.0.10.0 0.0.0.31 area 0
 network 10.0.10.32 0.0.0.15 area 0
 network 10.0.10.64 0.0.0.63 area 0
 network 10.0.10.128 0.0.0.7 area 0
R2 (Санкт-Петербург)
bash
router ospf 1
 router-id 2.2.2.2
 network 10.0.1.0 0.0.0.3 area 0
 network 10.1.1.0 0.0.0.255 area 0
R3 (Новосибирск)
bash
router ospf 1
 router-id 3.3.3.3
 network 10.0.2.0 0.0.0.3 area 0
 network 10.2.1.0 0.0.0.255 area 0
Пояснение: OSPF обеспечивает автоматический обмен маршрутами между всеми офисами. Все сети объединены в единую область Area 0.

3. Резервирование шлюза (HSRP)
R1-Main (Active)
bash
interface G0/0/1.10
 standby 10 ip 10.0.10.1
 standby 10 priority 150
 standby 10 preempt
!
interface G0/0/1.20
 standby 20 ip 10.0.10.33
 standby 20 priority 150
 standby 20 preempt
!
interface G0/0/1.30
 standby 30 ip 10.0.10.65
 standby 30 priority 150
 standby 30 preempt
!
interface G0/0/1.99
 standby 99 ip 10.0.10.129
 standby 99 priority 150
 standby 99 preempt
R1-Backup (Standby)
bash
interface G0/0/0.10
 standby 10 ip 10.0.10.1
 standby 10 priority 100
!
interface G0/0/0.20
 standby 20 ip 10.0.10.33
 standby 20 priority 100
!
interface G0/0/0.30
 standby 30 ip 10.0.10.65
 standby 30 priority 100
!
interface G0/0/0.99
 standby 99 ip 10.0.10.129
 standby 99 priority 100
Пояснение: HSRP создаёт виртуальный IP-адрес шлюза. R1-Main является активным, а R1-Backup – резервным. При выходе из строя основного маршрутизатора трафик мгновенно переключается на резервный.

4. DHCP (автоматическая выдача адресов)
R1-Main
bash
ip dhcp excluded-address 10.0.10.1 10.0.10.3
ip dhcp excluded-address 10.0.10.33 10.0.10.35
ip dhcp excluded-address 10.0.10.65 10.0.10.67
!
ip dhcp pool VLAN10_IT
 network 10.0.10.0 255.255.255.224
 default-router 10.0.10.1
 dns-server 10.0.99.11
!
ip dhcp pool VLAN20_ACCT
 network 10.0.10.32 255.255.255.240
 default-router 10.0.10.33
 dns-server 10.0.99.11
!
ip dhcp pool VLAN30_GUEST
 network 10.0.10.64 255.255.255.192
 default-router 10.0.10.65
 dns-server 10.0.99.11
Пояснение: DHCP-сервер автоматически выдаёт IP-адреса, шлюз и DNS-сервер устройствам в VLAN 10, 20, 30.

5. NAT (PAT) – выход в интернет
R1-Main
bash
access-list 1 permit 10.0.0.0 0.255.255.255
!
ip nat inside source list 1 interface GigabitEthernet0/0/0 overload
!
interface GigabitEthernet0/0/1
 ip nat inside
!
interface GigabitEthernet0/0/0
 ip nat outside
Пояснение: NAT (PAT) позволяет внутренним устройствам сети выходить в интернет через IP-адрес внешнего интерфейса маршрутизатора. Весь исходящий трафик транслируется в один публичный адрес.

6. Списки контроля доступа (ACL)
R1-Main
bash
ip access-list extended BLOCK_INTERNAL
 deny ip 10.0.10.32 0.0.0.15 10.0.10.0 0.0.0.31
 deny ip 10.0.10.64 0.0.0.63 10.0.10.0 0.0.0.31
 deny ip 10.0.10.64 0.0.0.63 10.0.10.32 0.0.0.15
 permit ip any any
!
interface G0/0/1.20
 ip access-group BLOCK_INTERNAL in
!
interface G0/0/1.30
 ip access-group BLOCK_INTERNAL in
Пояснение: ACL разграничивает доступ между отделами. Бухгалтерия (VLAN 20) и Гости (VLAN 30) не могут обращаться к ИТ-отделу (VLAN 10), а Гости также не могут обращаться к Бухгалтерии. Остальной трафик разрешён.

7. Брандмауэр (ASA 5505)
ASA
bash
interface Vlan1
 nameif inside
 security-level 100
 ip address 10.0.0.5 255.255.255.252
 no shutdown
!
interface Vlan2
 nameif outside
 security-level 0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
!
route outside 0.0.0.0 0.0.0.0 10.0.0.1
!
access-list inside_out extended permit ip any any
access-group inside_out in interface inside
Пояснение: ASA защищает периметр сети: внутренний интерфейс (inside) имеет уровень безопасности 100, внешний (outside) – 0. Разрешён весь исходящий трафик изнутри наружу, обеспечивая выход в интернет.

8. Примечание о GRE-туннелях
Первоначально планировалось реализовать связь филиалов через GRE-туннели (для VPN). Однако в используемой версии эмулятора Packet Tracer GRE работает некорректно с подинтерфейсами, поэтому было принято решение использовать маршрутизацию через OSPF, которая полностью решает задачу связности между офисами.

9. Ключевые особенности реализации
Единый Native VLAN 1000 на всех транках позволил избежать конфликтов STP.

HSRP обеспечивает отказоустойчивость шлюза в Москве.

NAT (PAT) настроен на R1-Main, так как ASA в Packet Tracer не поддерживает классический синтаксис nat/global.

DMZ (VLAN 100) изолирует публичные серверы (Web, DNS) от внутренней сети.
