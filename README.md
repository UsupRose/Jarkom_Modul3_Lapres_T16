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

-


















  

