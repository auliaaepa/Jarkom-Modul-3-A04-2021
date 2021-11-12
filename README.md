# Jarkom-Modul-3-A04-2021

**Nama Anggota Kelompok:**
* Julietta Anastasia Rodiah Boru Panjaitan (05111940000033)
* Aulia Eka Putri Aryani (05111940000044)
* Abdun Nafi’ (05111940000066)

Prefix IP: 10.1

## Soal 1 dan 2
Pada soal ini diminta untuk membuat topologi beserta konfigurasinya, dimana EniesLobby berperan sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server, dan Foosha sebagai DHCP Relay.

### Penjelasan
1. Buat topologi hingga diperoleh hasil sebagai berikut
![image](https://user-images.githubusercontent.com/76677130/140634384-e4b3ce67-6bd5-4398-9230-98b69324b4ea.png)
2. Ubah masing - masing konfigurasi network melalui `Configure` > `Edit network configuration` 
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



## Soal 2
Pada soal ini diminta untuk menjadikan `Foosha` sebagai `DHCP Relay`.

### Penjelasan
1. Buka **Foosha**
2. Install dhcp-relay
    ```
    apt-get update
    apt-get install isc-dhcp-relay -y
    ```
3. Prompt **server**
    ```
    10.1.2.4
    ```
4. Prompt **interface**
    ```
    eth1 eth2 eth3
    ```
    ![2a](https://user-images.githubusercontent.com/76677130/141493572-4171b8ac-d047-4cc3-96b1-87702963c902.PNG)
5. Cek hasil prompt pada file **/etc/default/isc-dhcp-relay**
    ![2b](https://user-images.githubusercontent.com/76677130/141493435-d3aa0445-20c7-4489-aa47-278d736dd63b.PNG)



## Soal 3, 4, 5, dan 6
Pada soal ini diminta untuk mengonfigurasikan IP client melalui `DHCP Server` pada `Jipangu`, dimana client yang melalui `Switch1` mendapatkan range IP dari `10.1.1.20 - 10.1.1.99` dan `10.1.1.150 - 10.1.1.169` sedangkan client yang melalui `Switch3` mendapatkan range IP dari `10.1.3.30 - 10.1.3.50`. Client mendapatkan DNS dari`EniesLobby` dan client dapat terhubung dengan internet melalui DNS tersebut. Lama waktu DHCP server meminjamkan alamat IP kepada client yang melalui Switch1 selama `6 menit` sedangkan pada client yang melalui Switch3 selama `12 menit`, dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama `120 menit`.

### Penjelasan
1. Buka **Jipangu**
2. Install dhcp-server
    ```
    apt-get update
    apt-get install isc-dhcp-server -y
    ```
2. Ubah **/etc/default/isc-dhcp-server** menjadi sebagai berikut
    ```
    INTERFACES="eth0"
    ```
    ![3a](https://user-images.githubusercontent.com/76677130/141496726-50c88c7f-d812-4137-80e1-0d850fdae79f.PNG)    
3. Ubah **/etc/dhcp/dhcpd.conf** menjadi sebagai berikut
    ```
    subnet 10.1.1.0 netmask 255.255.255.0 {
        range 10.1.1.20 10.1.1.99;
        range 10.1.1.150 10.1.1.169;
        option domain-name-servers 10.1.2.2;
        option routers 10.1.1.1;
        option broadcast-address 10.1.1.255;
        default-lease-time 360;
        max-lease-time 7200;
    }

    subnet 10.1.2.0 netmask 255.255.255.0 {

    }

    subnet 10.1.3.0 netmask 255.255.255.0 {
        range 10.1.3.30 10.1.3.50;
        option domain-name-servers 10.1.2.2;
        option routers 10.1.3.1;
        option broadcast-address 10.1.3.255;
        default-lease-time 720;
        max-lease-time 7200;
    }
    ```
    ![3b](https://user-images.githubusercontent.com/76677130/141496930-884ae2d1-461d-4dbe-8876-1a01d9e2edbe.PNG)
4. Restart dhcp-server
    ```
    service isc-dhcp-server restart
    ```
5. Buka semua client
6. Ubah **/etc/network/interfaces** menjadi berikut ini
    ```
    auto eth0
    iface eth0 inet dhcp
    ```
7. Restart node client pada gns3    
### Testing
* **Longuetown**
  ![3t1](https://user-images.githubusercontent.com/76677130/141497300-b90fce68-bde1-4833-b8c1-8d989c402e0d.PNG)
* **Alabasta**
  ![3t2](https://user-images.githubusercontent.com/76677130/141497345-23952171-abb3-4c63-adef-ec486e6c7d62.PNG)
* **TottoLand**
  ![3t3](https://user-images.githubusercontent.com/76677130/141497368-98332588-1725-479c-94ae-b620e83eea80.PNG)
* **Skypie**
  ![3t4](https://user-images.githubusercontent.com/76677130/141497409-26fafec9-cb0a-4fe9-84ad-6003fc8b4fd5.PNG)
* Cek lease time pada **/var/lib/dhcp/dhcpd.leases**
  ![3t5](https://user-images.githubusercontent.com/76677130/141497719-274281c4-b1d7-4ced-a0e8-120e0aee73f6.PNG)



## Soal 5
Pada soal ini diminta untuk membuat `DNS Server` pada `EniesLobby` dan menghubungkannya ke internet dengan domain **super.franky.a04.com**.

### Penjelasan
1. Buka **EniesLobby**
2. Install bind9
    ```
    apt-get update
    apt-get install bind9 -y
    ```
3. Ubah **/etc/bind/named.conf.local** menjadi sebagai berikut
    ```
    zone "super.franky.a04.com" {
        type master;
        file "/etc/bind/dns/super.franky.a04.com";
    };
    ```
    ![4a](https://user-images.githubusercontent.com/76677130/141502132-368e8c21-8662-47b7-823f-a7629f50edee.PNG)
4. Buat direktori **dns** pada **/etc/bind**
    ```
    mkdir /etc/bind/dns
    ```
5. Copy **/etc/bind/db.local** ke **/etc/bind/dns/super.franky.a04.com**
    ```
    cp /etc/bind/db.local /etc/bind/dns/super.franky.a04.com
    ```
6. Ubah **/etc/bind/dns/super.franky.a04.com** menjadi sebagai berikut
    ```
    $TTL    604800
    @       IN      SOA     super.franky.a04.com. root.super.franky.a04.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      super.franky.a04.com.
    @       IN      A       10.1.2.2        ; IP EniesLobby
    www     IN      CNAME   super.franky.a04.com.
    ```
    ![4b](https://user-images.githubusercontent.com/76677130/141502159-0e201fdf-960e-4288-9943-e89be81a8a86.PNG)
7. Ubah **/etc/bind/named.conf.options** menjadi sebagai berikut
    ```
    options {
        directory "/var/cache/bind";

        forwarders {
            192.168.122.1;
        };

        allow-query{ any; };

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    ![4c](https://user-images.githubusercontent.com/76677130/141502363-dd5340b6-856a-49fe-b767-3f185da53e07.PNG)
8. Restart bind
    ```
    service bind9 restart
    ```
### Testing
* Buka **Longuetown**
* Ketik command `ping -c 4 super.franky.a04.com`
* Ketik command `ping -c 4 google.com`
  ![4t](https://user-images.githubusercontent.com/76677130/141502946-e7e65517-0b80-4c6a-8af1-8ab2d5ae9afe.PNG)



## Soal 7
Pada soal ini diminta untuk mengubah IP `Skypie` menjadi tetap, yaitu `10.1.3.69`.

### Penjelasan
1. Buka **Jipangu**
2. Tambahkan **/etc/dhcp/dhcpd.conf** dengan berikut ini
    ```
    host Skypie {
        hardware ethernet 4a:00:cf:47:fe:b7;
        fixed-address 10.1.3.69;
    }
    ```
    ![7a](https://user-images.githubusercontent.com/76677130/141503925-1a50f37e-056f-4c4f-a9b0-9f7668a23538.PNG)
3. Restart dhcp-server
    ```
    service isc-dhcp-server restart
    ```
4. Buka **Skypie**
5. Tambahkan **/etc/network/interfaces** dengan berikut ini
    ```
    hwaddress ether 4a:00:cf:47:fe:b7
    ```
    ![7b](https://user-images.githubusercontent.com/76677130/141505388-8840190c-193f-44d3-82bb-696e100413cf.PNG)
6. Restart node Skypie pada gns3
### Testing
* **Skypie**
  ![7t](https://user-images.githubusercontent.com/76677130/141505491-30fb67b7-8f61-4db9-84e6-c6c2d5cfe010.PNG)



## Soal 8
Pada soal ini diminta untuk membuatkan proxy untuk `Longuetown` dengan nama **jualbelikapal.a04.com** dan port **5000**.

### Penjelasan
1. Buka **EniesLobby**
2. Tambahkan **/etc/bind/named.conf.local** dengan berikut ini
    ```
    zone "jualbelikapal.a04.com" {
        type master;
        file "/etc/bind/proxy/jualbelikapal.a04.com";
    };
    ```
    ![8aa](https://user-images.githubusercontent.com/76677130/141506328-070cfa98-6987-42ae-9543-2528823cea81.PNG)
3. Buat direktori **proxy** di dalam **/etc/bind**
    ```
    mkdir /etc/bind/proxy
    ```
4. Copy **/etc/bind/db.local** ke **/etc/bind/proxy/jualbelikapal.a04.com**
    ```
    cp /etc/bind/db.local /etc/bind/proxy/jualbelikapal.a04.com
    ```
5. Ubah **/etc/bind/proxy/jualbelikapal.a04.com** menjadi sebagai berikut
    ```
    $TTL    604800
    @       IN      SOA     jualbelikapal.a04.com. root.jualbelikapal.a04.com. (
                                  2         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      jualbelikapal.a04.com.
    @       IN      A       10.1.2.3        ; IP Water7
    ```
    ![8bb](https://user-images.githubusercontent.com/76677130/141507806-f899e672-b2f7-433f-9743-26304d21ec25.PNG)
6. Resart bind9
    ```
    service bind9 restart
    ```
7. Buka **Water7**
8. Install squid
    ```
    apt-get update
    apt-get install squid -y
    ```
9. Backup **/etc/squid/squid.conf**
    ```
    mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
    ```
10. Ubah **/etc/squid/squid.conf** menjadi sebagai berikut
    ```
    http_port 5000
    visible_hostname jualbelikapal.a04.com

    http_access allow all
    ```
    ![8a](https://user-images.githubusercontent.com/76677130/141507838-261fdfb0-c42b-4ce8-bb80-b3d85d474413.PNG)
11. Restart squid
    ```
    service squid restart
    ```
### Testing
* Buka **Longuetown**
* Install lynx
  ```
  apt-get update
  apt-get install lynx -y
  ```
* Ketikkan command `export http_proxy="http://jualbelikapal.a04.com:5000"`
* Ketikkan command `lynx its.ac.id`
  ![8t](https://user-images.githubusercontent.com/76677130/141508138-d3678b08-be7f-4e93-980f-fad07ba6f7e1.PNG)



## Soal 9
Pada soal ini diminta untuk memberi autentikasi user proxy dengan `enkripsi MD5` pada dua username, yaitu `luffybelikapalyyy` dengan password `luffy_yyy` dan `zorobelikapalyyy` dengan password `zoro_yyy`.

### Penjelasan
1. Buka **Water7**
2. Install apache2-utils
    ```
    apt-get update
    apt-get install apache2-utils -y
    ```
3. Membuat user dan password dengan enkripsi MD5
    ```
    htpasswd -b -c -m /etc/squid/passwd luffybelikapala04 luffy_a04
    htpasswd -b -m /etc/squid/passwd zorobelikapala04 zoro_a04
    ```
    ![9a](https://user-images.githubusercontent.com/76677130/141509778-0eafdc91-0c24-40d8-8fa4-9e6a0729acd2.PNG)
4. Cek user dan password pada **/etc/squid/passwd**
    ```
    cat /etc/squid/passwd
    ```
    ![9b](https://user-images.githubusercontent.com/76677130/141510117-bcb397ea-30a5-4c11-a1c7-56545536afbd.PNG)
5. Ubah **/etc/squid/squid.conf** menjadi sebagai berikut
    ```
    http_port 5000
    visible_hostname jualbelikapal.a04.com

    auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
    auth_param basic children 5
    auth_param basic realm Proxy
    auth_param basic credentialsttl 2 hours
    auth_param basic casesensitive on

    acl USERS proxy_auth REQUIRED

    http_access allow USERS
    http_access deny all
    ```
    ![9c](https://user-images.githubusercontent.com/76677130/141509839-9c5a6229-250d-4bc3-b12d-2784b8a6e875.PNG)
6. Restart squid
    ```
    service squid restart
    ```
### Testing
* Buka **Longuetown**
* Ketikkan command `lynx its.ac.id`
* Masukkan username
  ![9t1](https://user-images.githubusercontent.com/76677130/141510511-47abfa79-110d-4346-a04a-f4da59f3b08b.PNG)
* Masukkan password
  ![9t2](https://user-images.githubusercontent.com/76677130/141510564-129a197b-0fc3-4339-bb13-335c81f3d82b.PNG)
* Hasil
  ![9t3](https://user-images.githubusercontent.com/76677130/141510580-2c288b62-3e37-4d7d-924c-c43206687b94.PNG)



## Soal 10
Pada soal ini diminta untuk memberikan waktu akses pada proxy, yaitu setiap hari `Senin-Kamis` pukul `07.00-11.00` dan setiap hari `Selasa-Jum’at` pukul `17.00-03.00` keesokan harinya (sampai Sabtu pukul 03.00).

### Penjelasan
1. Buka **Water7**
2. Ubah **/etc/squid/squid.conf** menjadi sebagai berikut
    ```
    http_port 5000
    visible_hostname jualbelikapal.a04.com

    auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
    auth_param basic children 5
    auth_param basic realm Proxy
    auth_param basic credentialsttl 2 hours
    auth_param basic casesensitive on

    acl USERS proxy_auth REQUIRED
    acl AVAILABLE_TIME1 time MTWH 07:00-11:00
    acl AVAILABLE_TIME2 time TWHF 17:00-24:00
    acl AVAILABLE_TIME3 time WHFA 00:00-03:00

    http_access allow AVAILABLE_TIME1 USERS
    http_access allow AVAILABLE_TIME2 USERS
    http_access allow AVAILABLE_TIME3 USERS
    http_access deny all
    ```
    ![10a](https://user-images.githubusercontent.com/76677130/141511773-522d6d28-4fb5-4a81-8d12-fde88f6ffcea.PNG)
3. Restart squid
    ```
    service squid restart
    ```
### Testing
* Buka **Longuetown**
* Ubah date 
  ```
  date -s '21-11-12 14:00:00'
  ```
* Ketikkan command `lynx its.ac.id`
  ![10t](https://user-images.githubusercontent.com/76677130/141511950-dc218809-6145-4a71-a984-bb96c6412889.PNG)



## Soal 11
Pada soal ini diminta untuk redirecting **google.com** ke **super.franky.a04.com**, dimana `Web Server` super.franky.a04.com berada pada `Skypie`.

### Penjelasan
1. Buka **EniesLobby**
2. Ubah **/etc/bind/dns/super.franky.a04.com** menjadi sebagai berikut
    ```
    $TTL    604800
    @       IN      SOA     super.franky.a04.com. root.super.franky.a04.com. (
                                  3         ; Serial
                             604800         ; Refresh
                              86400         ; Retry
                            2419200         ; Expire
                             604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      super.franky.a04.com.
    @       IN      A       10.1.3.69       ; IP Skypie
    www     IN      CNAME   super.franky.a04.com.
    ```
    ![11a](https://user-images.githubusercontent.com/76677130/141514196-5bdf11b6-e4ac-4d11-83f1-d98424a7c179.PNG)
3. Restart bind9
    ```
    service bind9 restart
    ```
4. Buka **Skypie**
5. Install apache2 dan librarynya, serta php
    ```
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y    
    apt-get install php -y
    ```
6. Copy **/etc/apache2/sites-available/000-default.conf** ke **/etc/apache2/sites-available/super.franky.a04.com.conf**
    ```
    cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/super.franky.a04.com.conf
    ```
7. Ubah **/etc/apache2/sites-available/super.franky.a04.com.conf** menjadi sebagai berikut 
    ```
    <VirtualHost *:80>
        ServerName super.franky.a04.com
        ServerAlias www.super.franky.a04.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/super.franky.a04.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>
    ```
    ![image](https://user-images.githubusercontent.com/76677130/139525906-b27e018b-c5ba-4181-9c15-068620ff9b99.png)
8. Buat direktori **super.franky.a04.com** di dalam **/var/www**
    ```
    mkdir /var/www/super.franky.a04.com
    ```
9. Install wget dan unzip
    ```
    apt-get update
    apt-get install wget -y
    apt-get install unzip -y
    ```
10. Unduh file website
    ```
    wget https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip
    ```
11. Unzip file website
    ```
    unzip super.franky.zip
    ```
12. Pindahkan file website ke dalam **/var/www/super.franky.a04.com**
    ```
    mv super.franky/* /var/www/super.franky.a04.com
    ```
    ![image](https://user-images.githubusercontent.com/76677130/139525409-5cfb9e39-8ade-4c4f-b18b-0ad442fe0d1c.png)
13. Aktifkan konfigurasi
    ```
    a2ensite super.franky.a04.com.conf
    ```
14. Restart apache
    ```
    service apache2 restart
    ```
15. Buka **Water7**
16. Tambahkan **/etc/squid/squid.conf** dengan berikut ini
    ```
    acl GOOGLE dstdomain google.com
    deny_info http://super.franky.a04.com GOOGLE
    http_access deny GOOGLE
    ```
    ![11b](https://user-images.githubusercontent.com/76677130/141514119-df037d56-7bd4-4517-a34a-80e6cf061441.PNG)
17. Restart squid
    ```
    service squid restart
    ```
### Testing
* Buka **Longuetown**
* Ketikkan command `lynx google.com`
  ![11t](https://user-images.githubusercontent.com/76677130/141514624-2a9ab516-34cf-4842-b262-234720794d98.jpg)
* Masukkan username
  ![11t2](https://user-images.githubusercontent.com/76677130/141514746-26cc15a0-4492-4cac-9749-61feb068436a.PNG)
* Masukkan password
  ![11t3](https://user-images.githubusercontent.com/76677130/141514768-5db269e0-cff0-4c0e-85f8-9bf07918e2b4.PNG)
* Hasil
  ![11t4](https://user-images.githubusercontent.com/76677130/141514796-0ebdf127-4263-4996-ab5f-a174e69fb3c3.PNG)



## Soal 12
Pada soal ini diminta untuk memberikan akses `gambar` (.png, .jpg) kepada `Luffy` dan `sisanya` kepada `Zoro`. Ketika Luffy berhasil mendapatkan gambar, ia akan melihatnya dengan kecepatan `10 kbps`.

### Penjelasan
1. Buka **Water7**
2. Buat file **/etc/squid/acl-bandwidth.conf**
    ```
    auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

    acl LUFFY proxy_auth luffybelikapala06
    acl ZORO proxy_auth zorobelikapala06
    acl DOWNLOAD url_regex -i \.jpg$ \.png$

    delay_pools 1

    delay_class 1 1
    delay_parameters 1 1250/1250
    delay_access 1 allow LUFFY
    delay_access 1 deny ZORO
    delay_access 1 allow DOWNLOAD 
    delay_access 1 deny all
    ```
    ![12](https://user-images.githubusercontent.com/76677130/141515806-bf3349da-0d9a-4165-ad2b-63f2c0f6b2de.PNG)
3. Tambahkan **/etc/squid/squid.conf** dengan berikut ini
    ```
    include /etc/squid/acl-bandwidth.conf
    ```
    ![12b](https://user-images.githubusercontent.com/76677130/141515815-f368102d-941a-4d40-a947-2ef8dfde751c.PNG)
4. Restart squid
    ```
    service squid restart
    ```
### Testing
* Buka **Longuetown**
* Ketikkan command `lynx google.com`
* Masukkan username dan password
* Download gambar 
  (Tidak ada dokumentasi karena super.franky.a04.com diblock oleh provider internet)



## Soal 13
Pada soal ini diminta untuk memberikan akses kecepatan `tidak terbatas` kepada `Zoro`.

### Penjelasan
1. Buka **Water7**
2. Ubah **/etc/squid/acl-bandwidth.conf** menjadi sebagai berikut
    ```
    auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

    acl LUFFY proxy_auth luffybelikapala06
    acl ZORO proxy_auth zorobelikapala06
    acl DOWNLOAD url_regex -i \.jpg$ \.png$

    delay_pools 2

    delay_class 1 1
    delay_parameters 1 1250/1250
    delay_access 1 allow LUFFY
    delay_access 1 deny ZORO
    delay_access 1 allow DOWNLOAD
    delay_access 1 deny all

    delay_class 2 1
    delay_parameters 2 -1/-1
    delay_access 2 allow ZORO
    delay_access 2 deny LUFFY
    delay_access 2 deny all
    ```
    ![13a](https://user-images.githubusercontent.com/76677130/141516478-800c0f8e-8d5d-419f-8d52-854bd3266998.PNG)
3. Restart squid
    ```
    service squid restart
    ```
### Testing
* Buka **Longuetown**
* Ketikkan command `lynx google.com`
* Masukkan username dan password
* Download yang bukan gambar 
  (Tidak ada dokumentasi karena super.franky.a04.com diblock oleh provider internet)

## Kendala
Adapun kendala yang dialami selama pengerjaan soal praktikum modul ini adalah sebagai berikut.
* super.franky.a04.com diblock oleh provider internet ketika proxy dinyalakan, sehingga tidak dapat menguji kebenaran nomor 12 dan 13
