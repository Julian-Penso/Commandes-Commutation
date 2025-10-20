# 🖥️ Fichier de configuration – Postes & équipements réseau

## 1️⃣ Configuration de base (commutateurs et routeurs)
```bash
enable
configure terminal
hostname SW-A1
no ip domain-lookup
service password-encryption
enable secret cisco
banner motd # Accès réservé au personnel autorisé #
```

## 2️⃣ Configuration SSH
```bash
ip domain-name reseau.local
crypto key generate rsa general-keys modulus 1024
username admin secret cisco
line vty 0 15
transport input ssh
login local
```

## 3️⃣ Adresse IP de gestion (commutateur)
```bash
interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
ip default-gateway 192.168.1.1
```

## 4️⃣ Configuration des VLAN
```bash
vlan 10
name ADMIN
vlan 20
name RESEAUX
vlan 30
name SERVEURS
exit
```

## 5️⃣ Affectation des ports
```bash
interface range g0/1-10
switchport mode access
switchport access vlan 10
exit

interface g0/24
switchport mode trunk
switchport trunk allowed vlan all
```

## 6️⃣ EtherChannel (entre SW-D1 et SW-D2)
```bash
interface range g0/1-2
channel-group 1 mode active
exit
interface port-channel 1
switchport mode trunk
```
> Réf. cours Théo 6 – Redondance VLAN : LACP 802.3ad

## 7️⃣ STP et équilibrage
```bash
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary
```

## 8️⃣ Routage inter-VLAN (sur un MLS)
```bash
interface vlan 10
ip address 192.168.10.1 255.255.255.0
interface vlan 20
ip address 192.168.20.1 255.255.255.0
ip routing
```

## 9️⃣ HSRP – passerelle redondante (R1 et R2)
```bash
interface g0/0/0
ip address 192.168.1.2 255.255.255.0
standby 1 ip 192.168.1.1
standby 1 priority 110
standby 1 preempt
standby 1 timers 1 3
```
> Réf. Théo 4 – Redondance IP, HSRP tracking

## 🔟 DHCP (sur R1)
```bash
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool VLAN10
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
```

## 11️⃣ Sauvegarde et redémarrage
```bash
copy run start
reload
```
