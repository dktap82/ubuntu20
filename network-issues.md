# คู่มือตั้งค่า Network บน VirtualBox
## ตั้งค่า Adapter Network ที่ถูกเพิ่มเข้ามาตอนหลัง
**ตัวอย่าง**
ผลลัพธ์จากคำสั่ง `ip a`
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:32:84:f8 brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.101/24 brd 192.168.57.255 scope global dynamic enp0s3
       valid_lft 350sec preferred_lft 350sec
    inet6 fe80::a00:27ff:fe32:84f8/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 08:00:27:72:e2:d3 brd ff:ff:ff:ff:ff:ff
```
- enp0s8 คือ Adapter ที่ต้องให้ออกอินเทอร์เน็ตให้อย่างเดียว แต่เนื่องจาก enp0s8 ถูกเพิ่มเข้ามาทีหลังทำให้ OS ไม่ได้สั่งการทำงานของ enp0s8 จึงไม่ได้รับ IP
### ขั้นตอน
พิมพ์คำสั่ง 

1. `sudo mv /etc/netplan/*.yaml  /etc/netplan/01-netcfg.yaml` แล้ว Enter
2. `sudo nano /etc/netplan/01-netcfg.yaml` แล้ว Enter
จะแสดงผลครั้งแรกดังนี้
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
  version: 2
```
ให้ทำการเพิ่มคำสั่ง 2 บรรทัดด้านล่าง

> enp0s8:
> 
> dhcp4: true

จะได้ผลลัพธ์ ดังนี้
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: true
  version: 2
```
:warning: สองบรรทัดที่เพิ่มเข้ามาจำเป็นต้องจัดวรรคให้ตรงกันหรือให้ตรงกับในภาพ เพราะไม่เช่นนั้น OS จะไม่สามารถรับ IP ได้

3. `sudo reboot` แล้วกด Enter
4. หลังจาก OS Restart เสร็จ ใช้คำสั่ง `ip a` เพื่อตรวจสอบว่าได้รับ IP หรือไม่ ผลลัพธ์ที่ถูกต้องจะได้ ดังภาพ
```
cookiemilk@macvbox:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:32:84:f8 brd ff:ff:ff:ff:ff:ff
    inet 192.168.57.101/24 brd 192.168.57.255 scope global dynamic enp0s3
       valid_lft 327sec preferred_lft 327sec
    inet6 fe80::a00:27ff:fe32:84f8/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:72:e2:d3 brd ff:ff:ff:ff:ff:ff
    inet 10.0.3.15/24 brd 10.0.3.255 scope global dynamic enp0s8
       valid_lft 85528sec preferred_lft 85528sec
    inet6 fe80::a00:27ff:fe72:e2d3/64 scope link
       valid_lft forever preferred_lft forever
```
---
## License
~~DkTaP82~~&nbsp;&nbsp;:cookie:CookieMilk
