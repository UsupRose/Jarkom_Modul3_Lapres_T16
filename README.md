# **LAPRES_MODUL 3_JARKOM** 
-----------------------------------
## **KELOMPOK T_16**
## Bagus Farhan Abdillah (05311840000016)
## I Gede Pradhana Indra Widnyana (05311840000031)

-----------------------------------
```
 Teknis Pengerjaan
 1. Default memori UML adalah 64M, kecuali :
    a. SURABAYA : 256M
    b. MALANG : 160M
    c. MOJOKERTO : 128M
    d. TUBAN : 128M
 2. Menghitung dan menggunakan IP sesuai dengan NID Tuntap dan NID DMZ masing-masing kelompok
 3. IP Tuntap : NID_tuntap_tiap_kelompok + 1
 4. IP Interface Router SURABAYA :
    a. eth0 : NID_tuntap_tiap_kelompok + 2
    b. eth3 : NID_DMZ_tiap_kelompok + 1 
    c. eth1 : 192.168.0.1
    d. eth2 : 192.168.1.1
 5. IP Server (SUBNET 2) :
    MALANG    :  NID_DMZ_tiap_kelompok + 2
    MOJOKERTO : NID_DMZ_tiap_kelompok + 3
    TUBAN     : NID_DMZ_tiap_kelompok + 4
```
### Soal 1 
### Membuat topologi jaringan.

- Melakukan setting awal pembuatan UML. Untuk topologi yang ada pada gambar soal maka sintaksnya sebagai berikut.

![picture](https://cdn.discordapp.com/attachments/777146787336290354/782105765585879080/1.1_setting_awal_pembuatan_UML.JPG)

```
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.76.89 eth1=daemon,,,switch2 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem256M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Klien
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI umid-BANYUWANGI eth0=daemon,,,switch3 mem=64M &
```

- Kemudian kita lakukan konfigurasi interface pada setiap UML supaya bisa mendapatkan layanan dari DHCP. Setelah itu buka ``/etc/network/interfaces`` pada setiap client.
```
nano /etc/network/interfaces
```
- Lakukan pada SURABAYA tambahkan seperti pada gambar dibawah ini.
  
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782112359011319808/1.2_setting_interfaces_surabaya.JPG"  width="500" height="400">

- Lakukan pada MALANG tambahkan seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782117656764743680/1.3_setting_interfaces_malang.JPG"  width="500" height="400">

- Lakukan pada TUBAN tambahkan seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782118142137991219/1.4_setting_interfaces_tuban.JPG"  width="500" height="400">

- Lakukan pada MOJOKERTO tambahkan seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782118825935241256/1.5_setting_interfaces_mojokerto.JPG"  width="500" height="400">

- Lakukan pada BANYUWANGI tambahkan seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782118939831959562/1.6_setting_interfaces_banyuwangi.JPG"  width="500" height="400">

- Lakukan pada SIDOARJO tambahkan seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782118939831959562/1.6_setting_interfaces_banyuwangi.JPG"  width="500" height="400">

- Lakukan pada MADIUN tambahkan seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782119110028558356/1.8_setting_interfaces_madiun.JPG"  width="500" height="400">

- Lakukan pada GRESIK tambahkan seperti pada gambar dibawah ini.

<img src="https://media.discordapp.net/attachments/777146787336290354/782119211111677952/1.9_setting_interfaces_gresik.JPG"  width="500" height="400">

- Dan gambar berikut merupakan hasil UML setelah kita lakukan konfigurasi.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782120709005639700/1.10_hasil_uml.JPG" width="800" height="400">

### Soal 2
### SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.

- Setelah itu lakukan instalasi ISC-DHCP pada TUBAN. Lakukan package list pada TUBAN dengan command.
```
apt-get update
```
- Install ISC-DHCP dengan command.
```
apt-get install isc-dhcp-server
```

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782121992870559754/2.1_sukese_install_dhcp_tuban.JPG" width="500" height="400">

- Setelah itu lakukan konfigurasi interface DHCP pada TUBAN. Setelah itu buka konfigurasi interface dengan perintah.

```
nano /etc/default/isc-dhcp-server
```

- Kemudian tentukan interfaces ``eth0`` untuk diberikan layanan DHCP.

```
INTERFACE=eth0
```

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782124441828065280/2.2_setup_dhcp_server_tuban.JPG" width="500" height="400">

- Setelah itu lakukan konfigurasi DHCP pada TUBAN dengan command.

```
nano /etc/dhcp/dhcpd.conf
```

- Dan tambahkan script seperti pada gambar dibawah ini. Dan setelah itu lakukan restart servvice dengan perintah ``service isc-dhcp-server restart``

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782127245837598750/2.3_setup_dhcpd_conf_tuban.JPG" width="500" height="400">

- Kemudian lakukan penginstallan relay pada SURABAYA dengan command berikut.

```
apt-get install isc-dhcp-relay
```

- Setelah melakukan perintah tersebut akan muncul windows seperti dibawah ini. Dan masukkan ip TUBAN yaitu ``10.151.77.180``.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782135078381551626/2.4_dhcp_relay_surabaya_input_ip_tuban.JPG" width="500" height="400">

- Setelah memasukkan ip TUBAN, akan diminta untuk memasukkan konfigurasi interface seperti gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782135824452026419/2.5_setup_eth_relay_surabaya.JPG" width="500" height="400">

- Setelah itu proses instalasi isc-dhcp-relay pada surabaya telah berhasil seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782136457062121493/2.6_berhasil_membuat_relay_di_surabaya.JPG" width="500" height="400">

### Soal 3
### Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

- Lakukan konfigurasi subnet TUBAN untuk client GRESIK dan SIDOARJO dengan cara melakukan perintah.

```
nano /etc/dhcp/dhcpd.conf
```
- Tambahkan script berikut dan sesuai dengan gambar dibawah.
```
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.0.10 192.168.0.100;
    range 192.168.0.110 192.168.0.200;
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    option domain-name-servers 10.151.77.176 202.46.129.2;
    default-lease-time 300;
}
```
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782252647687127080/3.1_setup_subnet_tuban_untuk_client_gresik_sidoarjo.JPG" width="500" height="400">

- Dan lakukan service restart dengan perintah.
```
service isc-dhcp-server restart
```

- Kemudian lakukan konfigurasi pada interface client GRESIK dan SIDOARJO dengan perintah.
```
nano /etc/network/interfaces
```

- Dan isi konfigurasi sesuai pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782253907413303306/3.2_setting_interfaces_gresik.JPG" width="500" height="400">
<br />
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782254689123041400/3.3_setting_interfaces_sidoarjo.JPG" width="500" height="400">

- Setelah itu lakukan testing dengan perintah. 
```
cat /etc/resolv.conf
```
- Dan Berikut merupakan hasil dari kedua client yang telah dikonfigurasikan.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782256192033390602/3.4_hasil_subnet_1_gresik.JPG" width="500" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782256211285377024/3.5_hasil_subnet1_sidoarjo.JPG" width="500" height="400">

### Soal 4
### Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

- Lakukan konfigurasi subnet TUBAN untuk client BANYUWANGI dan MADIUN dengan cara melakukan perintah.

```
nano /etc/dhcp/dhcpd.conf
```
- Tambahkan script berikut dan sesuai dengan gambar dibawah.
```
subnet 192.168.0.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.70;
    option routers 192.168.1.1;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 10.151.77.176 202.46.129.2;
    default-lease-time 600;
    max-lease-time 7200;
}
```
<img src="https://media.discordapp.net/attachments/777146787336290354/782257709663780884/4.1_setup_subnet_tuban_untuk_client_banyuwangi_madiun.JPG" width="500" height="400">

- Dan lakukan service restart dengan perintah.
```
service isc-dhcp-server restart
```

- Kemudian lakukan konfigurasi pada interface client BANYUWANGI dan MADIUN dengan perintah.
```
nano /etc/network/interfaces
```

- Dan isi konfigurasi sesuai pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782258057257287690/4.2_setting_interfaces_banyuwangi.JPG" width="500" height="400">
<br />
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782258135008149514/4.3_setting_interfaces_madiun.JPG" width="500" height="400">

- Setelah itu lakukan testing dengan perintah. 
```
cat /etc/resolv.conf
```
- Dan Berikut merupakan hasil dari kedua client yang telah dikonfigurasikan.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782258234484326410/4.4_hasil_subnet_3_madiun.JPG" width="500" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782258310438584340/4.5_hasil_subnet_3_banyuwangi.JPG" width="500" height="400">

### Soal 5
### Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

- Lakukan konfigurasi DHCP Server pada TUBAN dengan perintah
```
nano /etc/dhcp/dhcpd.conf
````

- Dan lakukan konfigurasi sperti dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782460103239925810/6.1_setup_waktu_peminjaman_IP_subnet_1_dan_3.JPG" width="500" height="400">

- Dan berikut merupak hasil dns dari SIDOARJO, GRESIK, dan BANYUWANGI.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782463911583940648/5.2_hasil_dns_sidoarjo.JPG" width="500" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782464077984432168/5.3_hasil_dns_gresik.JPG" width="500" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782464180304478258/5.4_hasil_dns_banyuwangi.JPG" width="500" height="400">

### Soal 6
### Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

- Lakukan konfigurasi pada ``/etc/dhcp/dhcpd.conf`` dengan perintah.
```
nano /etc/dhcp.dhcpd.conf
```

- Dan lakukan konfigurasi seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782460103239925810/6.1_setup_waktu_peminjaman_IP_subnet_1_dan_3.JPG" width="500" height="400">

### Soal 7
### User autentikasi milik Anri memiliki format:
### - User : userta_yyy
### - Password : inipassw0rdta_yyy

- Lakukan penginstallan ``apache2-utils`` pada UML MOJOKERTO. Sebelumnya kalian sudah harus melakukan ``apt-get update``. Ketikkan:
```
apt-get install apache2-utils
```

- Setelah proses instalasi selesai. Lakukan pembuatan user login dengan perintah.
```
htpasswd -c /etc/squid/passwd jarkom203
```
 Ketikkan seperti pada gambar dibawah ini.
 
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782479843660136458/7.1_setup_username_dan_password_pada_mojokerto.JPG" width="500" height="400">

- Setelah itu edit konfigurasi squid menjadi.
```
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782481272613371945/7.2_setup_config_pada_mojokerto.JPG" width="500" height="400">

- Lakukan restart squid dengan perintah.
```
service squid restart
```

- Kemudian ubah pengaturan proxy browser. Gunakan IP MOJOKERTO sebagai host, dan isikan port 8080.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782482120823930940/7.3_setup_proxy_mojokerto.JPG" width="500" height="400">

- Kemudian cobalah untuk mengakses web (usahakan menggunakan mode incognito/private), akan muncul pop-up untuk login menggunakan username dan password yang telah kita buat.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782483115267981342/7.4_proxy_meminta_login_username_dan_password.JPG" width="800" height="400">

- Dan gambar dibawah ini merupakan contoh berhasil dari membuat username dan password serta mengakses website.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782483931483340810/7.5_berhasil_membuat_username_dan_password_serta_mengakses_website.JPG" width="800" height="400">

### Soal 8
### Setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak    dapat mengakses jaringan internet dengan proxy tersebut.

- Buatlah file baru pada UML MOJOKERTO bernama acl.conf di folder squid
```
nano /etc/squid/acl.conf
```

- Dan isikan seperti gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782485748992704542/8.1_setup_acl_pada_mojokerto.JPG" width="500" height="400">

- Setelah itu simpan filenya. Dan buka file squid.conf dengan perintah
```
nano /etc/squid/squid.conf
```
- Setelah itu lakukan konfigurasi seperti pada gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782486913449328650/8.2_setup_squid_config_pada_mojokerto.JPG" width="500" height="400">

- Kemudian restart squid dengan perintah.
```
service squid restart
```

- Cobalah untuk mengakses web http://its.ac.id (usahakan menggunakan mode incognito/private). Akan muncul halaman error jika mengakses diluar waktu yang telah ditentukan.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782493844474167346/8.3_setelah_dicoba_meminta_login.JPG" width="900" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782494043057291264/8.4_berhasil_akses_karena_sesuai_waktu.JPG" width="900" height="400">

### Soal 9
### Setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA.

- Lakukan konfigurasi acl pada MOJOKERTO dengan command. 
```
nano /etc/squid/acl.conf
```

- Isikan konfigurasi sesuai dengan gambar dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782495683633872906/9.1_setup_acl_pada_mojokerto.JPG" width="500" height="400">

- Kemudian lakukan konfigurasi squid.conf dengan command.
```
nano /etc/squid/squid.conf
```
- Lalu isikan konfigurasi seperti dibawah ini.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782505554617958400/9.2_setup_squid_config_pada_mojokerto.JPG" width="500" height="400">

- Kemudian restart squid dengan perintah.
```
service squid restart
```

- Cobalah untuk mengakses web http://its.ac.id (usahakan menggunakan mode incognito/private). Akan muncul halaman error jika mengakses diluar waktu yang telah ditentukan.

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782493844474167346/8.3_setelah_dicoba_meminta_login.JPG" width="900" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782494043057291264/8.4_berhasil_akses_karena_sesuai_waktu.JPG" width="900" height="400">

### Soal 10
### Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA.

- Lakukan konfigurasi redirect pada squid.conf dengan command.
```
nano /etc/suqid/squid.conf
```
- Dan lakukan konfigurasi seperti gambar dibawah ini

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782511183000633344/10.1_setup_redirect_pada_config_squid.JPG" width="500" height="400">

- Berikut merupakan hasil membuat redirect ke monta setiap mengakses google.com

<img src="https://cdn.discordapp.com/attachments/777146787336290354/782483115267981342/7.4_proxy_meminta_login_username_dan_password.JPG" width="900" height="400">
<br>
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782511868124069918/10.3_berhasil_membuat_redirect_ke_monta_setiap_mengakses_google.JPG" width="900" height="400">

### Soal 11
### Bu Meguri meminta Anri untuk mengubah error page default squid menjadi seperti berikut
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782513406237343744/unknown.png" width="900" height="400">








  



