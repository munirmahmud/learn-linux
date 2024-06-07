# RHCSA Class 24: Network Configuration

### Networking Address

নেটওয়ারকিং এ আমরা দুই ধরণের অ্যাড্রেস ব্যাবহার করে থাকি।

- 1. Logical Address - L3 Address, IP Address, Network Address

  1) IPv4 (32 Bits)
  2) IPv6 (64 Bits)

- 2. Physical Address - L2 Address, MAC Address, Ethernet Address

- 3. Loopback Address যেকোনো OS এর সাথে থাকে।

প্রত্যেকটা ডিভাইসের সাথে MAC Address থাকবেই। এর সাথে আরেকটা অ্যাড্রেস থাকে তাকে বলে `Loopback address` বলে। একটা ডিভাইস থেকে আরেকটা ডিভাইসের সাথে যোগাযোগ করার জন্য ইউনিক আইডি দরকার যাকে IP Address বলে। IP Address এর কাজ হল ডিভাইস আইডেন্টিফাই করা।

### IPv4 Address

- 32 Bits (4 Bytes) - 1101010101010101010110000101010111
- 4 Parts 4 Octets (1st.2nd.3rd.4th) - 11010101.01010101.01010000.10101111
- Seperated by dot (.)
- Decimal Format (0-9)
- Example: 203.112.194.243
- Total Address: IPv4 (32bits), 2^32=2^8.2^8.2^8.2^8 (256x256x256x256) => 2^32 = 4,294,967,296
- 8 bit এর ম্যাক্সিমাম ডেসিম্যাল ভ্যালু = 2^8 = 256 (0-255)

### IPv4 Classification

- 1. Class A - 0-127 -----> (শুধুমাত্র প্রথম ঘর অর্থাৎ `1st Octet` এর জন্য প্রযোজ্য।)
- 2. Class B - 128-191 -------------> ||
- 3. Class C - 192-223 -------------> ||
- 4. Class D - 224-239 <----- Multicast Application
- 5. Class E - 240-255 <----- Reserved/Experimenta

নোটঃ কোন IP কোন Class এ পড়েছে এটা বের করা যাবে `1st Octet` দেখে।

#### Test

- 203.112.194.243
- 224.0.0.10
- 191.224.242.20
- 135.253.104.25

### IPv4 (TCP/IP Communication) - Unicast & Broadcast IP

- Class A: 0.0.0.0 - 127.255.255.255
- Class B: 128.0.0.0 - 191.255.255.255
- Class C: 192.0.0.0 - 223.255.255.255

### Types of IPv4 Address:

- 1. Public IP (Globally Routable & Paid, lease, Real)
- 2. Private IP (Non Routable & Free)
- 3. Localhost IP - 127.0.0.0/8 - Loopback IP (IPv4) Address প্রতিটি সিস্টেমে এটা ডিফল্ট থাকে।
- 4. Reserved IP - 169.254.0.1-169.254.255.254 (APIPA) - যখন স্ট্যাটিক কনফিগার করা না থাকে এবং `DHCP` না থাকে তখন এটা সেট হয়।
- 5. Documentation - 198.51.100.0/24

- `DHCP - Dynamic Host Configuration Protocol`
- `APIPA - Automatic Private IP Addressing`
- `CIDR - Classless Inter-Domain Routing` is an IP address allocation method that improves data routing efficiency on the internet. Every machine, server, and end-user device that connects to the internet has a unique number, called an IP address, associated with it.

নোটঃ প্রতিটি `IP Address` এর সাথে আমরা একটা ভ্যালু বসাই যেটাকে বলে `Subnet Mask` এবং এটা লিখে এভাবে `192.168.146.147/24` প্রকাশ করি। এটাকে বলে `CIDR notation` এর মাধ্যমে আমরা নেটওয়ার্কের সাইজ নির্ধারণ করি। একটা নেটওয়ার্ক কত বড় হবে তা নির্ভর করে এই `Subnet Mask` এর উপর।

- 127.0.0.0/8 - 127.0.0.0/255.0.0.0
- 170.0.0.0/16 - 170.0.0.0/255.255.0.0
- 192.0.0.0/24 - 192.0.0.0/255.255.255.0

নোটঃ ৮ বিটের ম্যাক্সিমাম ডেসিম্যাল ভ্যালু ২৫৫ অর্থাৎ `2^32 = 255`

### Private Address Range :

- Class A: 10.0.0.0 - 10.255.255.255
- Class B: 172.16.0.0 - 172.31.255.255 <---- `RHCSA Exam Block` এক্সামে সাধারণত এই ব্লকের প্রশ্ন আসে।
- Class C: 192.168.0.0 – 192.168.255.255

### Linux Network Management

Windows Operating System এ যখন IP বসানো হয়, তখন নিচের নামের মত বিভিন্ন ইন্টারফেইস দেখা যায়।

Control Panel > Network and Internet > View Network Connections

- Windows NIC1: Ethernet1
- Windows NIC2: Ethernet2

`[root@devs ~]# ip link`

- Interface Name = lo,en,wl,vir (lo=loopback, en=ethernet, wl=wireless, vir-br=Virtual Bridge)
- Status = State UP
- Link Speed = 1000
- MAC Address = Ether
- MTU = 1500 (সরবোচ্চ কত সাইজের ফ্রেম সেন্ড/রিসিভ করতে পারে)

`MTU - maximum transmission unit` In networking, maximum transmission unit (MTU) is a measurement representing the largest data packet that a network-connected device will accept.

`[root@devs ~]# ip addr show`

- inet = IPv4 Address
- inet6 = IPv6 Address (Link Local Address)
- brd = broadcast Address
- netmask = Subnet mask
- broadcast = Broadcast Address
- ether = MAC Address
- Speed = 1000

নোটঃ `ifconfig` কমান্ডের ক্ষেত্রে `net-tools` নামের প্যাকেজ (RPM) ইন্সটল থাকতে হবে।

`[root@devs ~]# ifconfig`

`[root@devs ~]# ifconfig ens160` sepecific LAN (br0, eth0, ens33, enp2s0, ens192)

### Linux (RHEL/CentOS-9)

- Ethernet: enp, eno, ens (start with 'en')
- Legacy: eth0
- Virtual/sub Interface: enp2s0:1 or ens160:1, - ens33:1, eth0:1
- loopback: lo
- Bridge : br0, vbri0
- Wireless: wl

### Check Physical Connectivity

```
[root@devs ~]# ip link
state "UP" or State "Down"
[root@devs ~]# ethtool enxxx (interface name)
```

### Host Name Configure (Desktop Machine)

```
[root@devs ~]# hostname
devs.example.com

[root@devs ~]# hostnamectl status
[root@devs ~]# hostnamectl set-hostname node1.example.com
[root@devs ~]# exec bash

[root@node1 ~]# cat /etc/hostname
node1.example.com
[root@node1 ~]# hostname
```

### Network Card Enable and Disable

```
[root@node1 ~]# ip link                  ----> প্রথমে ইন্টারফেসের নাম দেখে নিতে হবে।
[root@node1 ~]# ip link set ens160 down  ----> নেটওয়ার্ক কার্ড ডিজেবল করা হয়েছে।
[root@node1 ~]# ip link (Showing DOWN)   ---->  নেটওয়ার্ক স্ট্যাটাস দেখার জন্য।
[root@node1 ~]# ip link set ens160 up    ----> নেটওয়ার্ক কার্ড ইনেবল করা হয়েছে।
```

### Internet Gateway

```
[root@devs ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    100    0        0 ens160
192.168.0.0     0.0.0.0         255.255.255.0   U     100    0        0 ens160
```

নোটঃ `route -n` কমান্ড কাজ না করলে `ip route` কমান্ড ব্যবহার করা যেতে পারে।

### DNS দেখার জন্য

```
[root@devs ~]# cat /etc/resolv.conf
nameserver 172.25.11.254
nameserver 4.2.2.2
```

### External network check

`ping 8.8.8.8`

`ping www.google.com`

`ping -c 4 8.8.8.8` নির্দিষ্ট সঙ্খক `ping` দেখতে চাইলে

নোটঃ `Bridge Interface` হল ফিজিক্যাল কানেকশন। (Vmware)

### New Default GW Add (Optional) :

```
[root@node1 ~]# route add default gw 172.25.11.254
[root@node1 ~]# route -n
[root@node1 ~]# ping 8.8.8.8
[root@node1 ~]# route del default gw 172.25.11.254
[root@node1 ~]# route -n
[root@node1 ~]# ping 8.8.8.8
```

### IP Client Configure

#### আমরা দুইভাবে IP Address configure করতে পারি।

- Static (Manually)
- DHCP (Automatically IP configure)

বিভিন্ন Utility (Tools) ব্যবহার করে Static IP configure করা যায়।

- 1. nmtui - NetworkManager Text user interface
- 2. nmcli - NetworkManager Command line Interface
- 3. GUI - Graphical User Interface
- 4. vim - Using Editor Tools
- 5. yml - using YML File

লিনাক্সে নেটওয়ার্ক সম্পর্কিত সার্ভিস `NetworkManager` ডেমন (Daemon) দিয়ে ম্যানেজ করা হয়। সুতরাং, IP বসানোর আগে NetworkManager সার্ভিসটি ঠিকমতো রান করতেছে কিনা সেটা দেখে নিতে হবে। রান না করলে নিচের কমান্ড দিয়ে এটাকে `start` এবং `enable` করে নিতে হবে।

```
systemctl status NetworkManager.service
systemctl restart NetworkManager.service
systemctl enable NetworkManager.service
```

### How to get IP address?

`ip address show / ifconfig` দিয়ে IP address এর বিস্তারিত জানা যায়।

`ip address show ens160 / ifconfig ens160` ইন্টারফেইসের নাম দিয়েও নির্দিষ্ট IP address এর সম্পর্কে জানা যায়।

`ifconfig` কমান্ডের মাধ্যমে netmask সহ নেটওয়্যারকের ব্যাপারে জানা যায়।

```
inet = IPv4 Address
inet6 = IPv6 Address
brd = broadcast Address
mtu = maximum transmission unit (1500)
netmask = Subnet mask
broadcast = Broadcast Address
```

হার্ডওয়্যারের ভ্যারিয়েশনের জন্য ইন্টারফেইস এর নামেরও ভিন্নতা দেখা যায় যেমনঃ `ens33, ens160, ens244, ens256, emp1s0, eth0, eth1, eno1 eno2, lan1` ইত্যাদি।

লোকাল মেশিনে `Host OS` হিসেবে `Linux OS` দেয়া থাকলে এবং এর Network Connection যদি wifi হয় তাহলে ইন্টারফেইস নাম `wlan0, wlan1` ইত্যাদি নামে থাকতে পারে।

`ip addr / ip addr show` Network সম্পর্কিত তথ্য পাওয়া যায়।

সংক্ষেপে `ip a s` লেখা যায়। আবার নির্দিষ্ট ইন্টারফেইস দেখতে চাইলে `ip a s ens160`

`ip route` এর মাধ্যমে Gateway IP address সম্পর্কে জানতে পারি। সংক্ষেপে `ip r` লেখা যায়।

- SNAT &rarr; Source Network Address Translation
- DNAT &rarr; Destination Network Address Translation

`/etc/resolve.conf` এই ফাইলে DNS তথ্য থাকে।

### Network Manager

`nmcli stands for Network Manager Command Line Interface` nmcli is used to create, display, edit, delete, activate, and deactivate network connections, as well as control and display network device status.

`nmcli device show` এই কমান্ডটি নেটওয়ার্ক ডিভাইসের সকল তথ্যের একটা লিস্ট প্রদান করে।

`nmcli` দিয়ে ট্যাব প্রেস করলে আরো কিছু অপশন পাওয়া যাবে।

### CentOS 7 Network Configuration File

`/etc/sysconfig/network-scripts/ifcfg-ens33` এই ফাইলে নেটওয়ার্ক কনফিগারেশন তথ্য থাকে। এই ফাইলে কোন রকম চেঞ্জ করলে তার জন্য নেটওয়ার্কে রিস্টার্ট দিতে হবে।

নেটওয়ার্ক রিস্টার্ট দেয়ার কমান্ড।

```
systemctl restart network
or
systemctl restart NetworkManager
or
ifdown ens33` and `ifup ens33
or
nmcli networking off ens33 & nmcli networking up ens33
```

ISO ইন্সটল করার সময় IP address ম্যানুয়ালি কনফিগার না করলে নিচের ভ্যারিয়েবলগুলো এবং তার ভ্যালুগুলো অ্যাড করে দিতে হবে।

```
BOOTPROTO="(auto/dhcp)/(static/manual)/none" (auto & dhcp same) লিনাক্সে `auto/manual/none` prefer করে বেশি।
IPADDR=""
PREFIX="24"
#NETMASK="255.255.255.0"
GATEWAY=""
DNS1=""
DNS2=""
IPV6_PRIVACY="NO"
```

VMware Workstation এর মেনু থেকে Edit > Virtual Network Editor. এখানে নেটওয়ার্ক টাইপ `Host Only` মানে হলো সেইম ব্লকের IP তে ইউজ করার জন্য। আর NAT মানে হলো অন্যব্লকের IP তেও access নেয়া যায়।

`ifdown ens33` নেটওয়ার্ক ইন্টারফেইসের নাম দিয়ে ঐ নেটওয়ার্ককে Disconnect/Diactivate করা যায়।

`ifup ens33` নেটওয়ার্ক ইন্টারফেইসের নাম দিয়ে ঐ নেটওয়ার্ককে Connect/Activate করা যায়।

`/etc/resolve.conf` ফাইলে একাধিক DNS নাম দেয়া যায় কিন্তু কাজ করবে অনলি ৩ টা।

### RHEL 9 Network Configuration

`/etc/NetworkManager/system-connections/` নেটওয়ার্ক কনফিগারেশন ফাইল। ডিরেক্টলি এই ফাইলে কোন রকম চেঞ্জ না করাই ভাল। সব ধরনের চেঞ্জেস কমান্ডের মাধ্যমে করা ভাল।

`nmcli connection show` কমান্ডের মাধ্যমে নেটওয়ার্কের লিস্ট দেখতে পারি।

```
nmcli connection modify ens160 ipv4.addresses 192.168.0.110/24
nmcli connection modify ens160 ipv4.gateway 192.168.0.2
nmcli connection modify ens160 ipv4.dns 8.8.8.8
nmcli connection modify ens160 ipv4.method manual
nmcli connection modify ens160 autoconnect yes
```

এখন নেটওয়ার্ক ম্যানেজারকে রিস্টার্ট দিতে হবে।

```
nmcli networking off ens160 (Device Name / con-name)
nmcli networking on ens160 (Device Name / con-name)
```

অথবা

```
nmcli connection down ens160 (<con-name>)
nmcli connection up ens160 (<con-name>)
```

`nmcli connection up <con-name>` শুধুমাত্র up ইউজ করেও নেটওয়ার্ক রিলোড করা যায়।

`ifconfig ens160` কমান্ড দিয়ে চেক করা যে IP চেঞ্জ হয়েছে কিনা।

আমরা চাইলে উপরের সকল কমান্ড একসাথে রান করতে পারি।

```bash
nmcli connection modify ens160 ipv4.addresses 192.168.0.110/24 ipv4.gateway 192.168.0.2 ipv4.dns 1.1.1.1,4.2.2.2,8.8.8.1 ipv4.method manual connection.autoconnect yes
```

`nmcli connection delete Wired Connection 1` কানেকশন ডিলিট করার জন্য।

নতুন নেটওয়ার্ক কার্ড অ্যাড করে ঐ কানেকশনকে `Wired Connection 1` ডিলিট করে তারপরে নিচের কমান্ড দিলে কানেকশন নাম চেঞ্জ করা যায়।

কানেকশন আগে অ্যাড করে তারপরে আইপি এবং অন্য তথ্য অ্যাড করা যায় আবার সবগুলো কমান্ড একসাথে রান করেও করা যায়।

`nmcli connection add con-name private-net type ethernet ifname ens192`

```bash
nmcli connection add con-name private-net type ethernet ifname ens192 ipv4.addresses 192.168.0.110/24 ipv4.gateway 192.168.0.2 ipv4.dns 1.1.1.1,4.2.2.2,8.8.8.1 ipv4.method manual connection.autoconnect yes
```
