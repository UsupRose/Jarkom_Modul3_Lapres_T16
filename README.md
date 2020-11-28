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
### Soal 1 :
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

- Kemudian kita lakukan konfigurasi interface pada setiap client supaya bisa mendapatkan layanan dari DHCP. Setelah itu buka ``/etc/network/interfaces`` pada setiap client.
```
nano /etc/network/interfaces
```
  Lalu tambahkan seperti pada gambar dibawah ini.
  
<img src="https://cdn.discordapp.com/attachments/777146787336290354/782112359011319808/1.2_setting_interfaces_surabaya.JPG" alt="ini gambar kontol" width="100" height="200">
  

