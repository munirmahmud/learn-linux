# RHCSA Class 24: Network Configuration

আমরা দুইভাবে IP Address configure করতে পারি।

- Static (Manually)
- DHCP (Automatically IP configure)

বিভিন্ন Utility (Tools) ব্যবহার করে Static IP configure করা যায়।

- 1. nmtui - NetworkManager Text user interface
- 2. nmcli - NetworkManager Command line Interface
- 3. GUI - Graphical User Interface
- 4. vim - Using Editor Tools
- 5. yml - using YML File

লিনাক্সে নেটওয়ার্ক সম্পর্কিত সার্ভিস `NetworkManager` ডেমন (Daemon) দিয়ে ম্যানেজ করা হয়। সুতরাং, IP বসানোর আগে NetworkManager সার্ভিসটি ঠিকমতো রান করতেছে কিনা সেটা দেখে নিতে হবে। রান না করলে নিচের কমান্ড দিয়ে এটাকে `start` এবং `enable` করে নিতে হবে।

`systemctl status NetworkManager.service`

`systemctl restart NetworkManager.service`

`systemctl enable NetworkManager.service`

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

`nmcli connection modify ens160 ipv4.addresses 192.168.0.110/24`

`nmcli connection modify ens160 ipv4.gateway 192.168.0.2`

`nmcli connection modify ens160 ipv4.dns 8.8.8.8`

`nmcli connection modify ens160 ipv4.method manual`

`nmcli connection modify ens160 autoconnect yes`

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

<!-- Done -->
