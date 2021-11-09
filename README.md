# Jarkom-Modul-3-A04-2021

## Topology
![image](https://user-images.githubusercontent.com/76677130/140634384-e4b3ce67-6bd5-4398-9230-98b69324b4ea.png)

* Foosha
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
    address 10.1.1.1
    netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
    address 10.1.2.1
    netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
    address 10.1.3.1
    netmask 255.255.255.0 
  ```
* Longuetown
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.1.2
    netmask 255.255.255.0
    gateway 10.1.1.1
  ```
* Alabasta
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.1.3
    netmask 255.255.255.0
    gateway 10.1.1.1
  ```
* EniesLobby
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.2.2
    netmask 255.255.255.0
    gateway 10.1.2.1
  ```
* Water7
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.2.3
    netmask 255.255.255.0
    gateway 10.1.2.1
  ```
* Jipangu
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.2.4
    netmask 255.255.255.0
    gateway 10.1.2.1
  ```
* TottoLand
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.3.2
    netmask 255.255.255.0
    gateway 10.1.3.1
  ```
* Skypie
  ```
  auto eth0
  iface eth0 inet static
    address 10.1.3.3
    netmask 255.255.255.0
    gateway 10.1.3.1
  ```
