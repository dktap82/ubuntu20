# คู่มือตั้งค่า Network บน VirtualBox
## การเพิ่ม Adapter LAN หรือ Adapter Network
:white_check_mark: เมนูภาษาอังกฤษ | :white_check_mark: ใช้ได้ทั้ง Windows ,Linux และ macOS
### ขั้นตอน
1. เลือก Guest OS ที่ต้องการหรือ VM ที่การเพิ่ม โดยการคลิกขวาเลือก `Settings...`
2. จะปรากฏหน้าต่าง Settings ขึ้นมา ไปยังแทบเมนู `Network` เลือก `Adapter 2` หรือ Adapter ไหนก็ได้ สูงสุด 4 Adapter
3. ตัวอย่างนี้จะทำการเลือก `Adapter 2` แล้วติ๊กถูกที่ `Enable Network Adater`
4. ที่หัวข้อ `Attached to:` หมายถึงจะให้ Adapter นี้ทำงานในรูปแบบไหน โดยจะใช้ประจำอยู่ 4 เมนู สำหรับโปรแกรมเมอร์ รายละเอียดดังนี้
    - Not attached ไม่ระบุหรือไม่ให้มีการ เปรียบเสมือนช่องแลนที่ไม่ได้เสียบสายแลน หากมองอีกมุมเปรียบเสมือนการจองสล็อตแลนไว้เฉย ๆ
        - หัวข้อ `Name:` ไม่มีตัวเลือก
    - Nat เปรียบเสมือนการเสียบสายแลน โดยสามารถออกเน็ตได้ โดยจะได้ IP หมายเลข 10.0.2.0/24
        - หัวข้อ `Name:` ไม่มีตัวเลือก
    - Bridged Adapter เปรียบเสมือนเสียบสายแลน แต่จะได้ IP เบอร์เดียวกับเครื่องเรา เป็นการที่ VM ใช้ Adapter Network ร่วมกับเครื่องเรา
        - หัวข้อ `Name:` มีตัวเลือก โดยจะเป็นรายชื่อ Adapter Network ในเครื่องเรา หากเป็นเครื่อง Notebook จะมีมากกว่า 1 เพราะมี Adapter Wireless(WiFi)
    - Host-only Adapter เปรียบเสมือน Adapter Network ที่จะทำให้ VM และเครื่องเราสามารถมองเห็นกันและกัน แต่จะไม่สามารถออกเน็ตได้ โดยเมนูส่วนนี้สามารถจัดสรรเองได้ ไม่ว่าจะเป็นการเลือก IP ได้ด้วยตัวเอง หรือ ตั้งค่าให้จ่ายหรือไม่ให้จ่าย IP ด้วยก็ได้
        - หัวข้อ `Name` มีตัวเลือก โดยจะเป็นชื่อ Adapter ที่จะใช้งานร่วมกันกับ VM และเครื่องเรา โดยจะมี Adapter เริ่มต้นที่ชื่อ `VirtualBox Host-Only Ethernet Adapter` จ่าย IP อัตโนมัติหมายเลข 192.168.56.1/24
            - หากต้องการเพิ่ม Host-Only Adapter ไปที่เมนู `File` -> `Host Network Manager...`
            - จะปรากฏหน้าต่าง `Host Network Manager` โดยจะมีเมนู 3 เมนูใหญ่ คือ "Create" คือการเพิ่ม | "Remove" คือการลบ | "Properties" คือรายละเอียดของ Host-Only Adapter สามารถตั้งค่าเพิ่มเติมได้ สำหรับ Host-Only Adapter ที่มีอยู่ก่อนแล้ว

    ***จบหัวข้อนี้***

### ในกรณีต้องการต้องการให้ Adapter Network ตัวใดตัวหนึ่ง เอาไว้ Remote SSH สามารถทำได้ ดังนี้
1. ทำการเพิ่ม Adapter Network มากกว่า 1 ตัว โดยให้ตัว Adapter Network ตัวใดตัวหนึ่งเป็น `Nat` และอีกตัวหนึ่งเป็น `Host-Only Ethernet Adapter`

:octocat: *โดยการทำงานนี้ เราจะใช้ IP ของ `Host-Only Ethernet Adapter` Remote SSH เข้าไป VM แต่จะไม่สามารถออกเน็ตได้ แต่เรามี Adapter ที่เราเซ็ทเป็น `Nat` ซึ่งเอาไว้ออกเน็ตโดยเฉพาะจะทำ VM ของเราจะสามารถใช้งานเน็ตได้ปกติ*
## ตั้งค่า Adapter ตัวที่ 2 หรือ Adapter ที่ถูกเพิ่มเข้ามาในภายหลัง ให้รับ IP Address
:white_check_mark: เมนูภาษาอังกฤษ | :white_check_mark: ใช้ได้ทั้ง Windows ,Linux และ macOS

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
~~DkTaP82~~ :cookie:CookieMilk
