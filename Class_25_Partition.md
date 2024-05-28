# RHCSA Class 25: File System & Partition Management

আমরা যখন RHEL 9/Alma Linux অপারেটিং সিস্টেম ইন্সটল করেছি, তখন চার টা পার্টিশন করেছিলাম। এবং এগুলোর সাইজ যথাক্রমে নিচে উল্লেখ করা হল।

- `/boot/efi - 200 MiB` GPT ভিত্তিক পার্টিশনের ক্ষেত্রে লাগবে
- `/boot - 1024 MiB`
- `/  - 20 GiB`
- `swap - 2 GiB`

`lsblk` `List of Block Device` এই কমান্ড দিয়ে সিস্টেমের সকল পার্টিশন লিস্ট দেখা যাবে।

`lsblk -f` এই কমান্ড দিয়ে সিস্টেমের সকল পার্টিশন এবং পার্টিশন আইডি, এবং কি ধরনের ফাইল সিস্টেম ব্যাবহার করা হয়েছে তা লিস্ট আকারে দেখা যাবে।

Linux/Windows System এ দুইভাবে পার্টিশন টেবিল তৈরি করা যায়।

- 1. (MBR) - BIOS/Legacy Firmware > MBR (Master Boot Record) - Linux এ `dos` নামে থাকে।
- 2. (GPT) - UEFI Firmware > GPT (GUID Partition Table) + MBR - Linux এ `gpt` নামে থাকে।

`fdisk -l ` সিস্টেমের পার্টিশন টেবিল তথা পার্টিশন সম্পর্কিত সকল তথ্য দেখা যাবে। যেমনঃ পার্টিশন টাইপ, সেক্টরস, ফাইল সাইজ, ফাইল টাইপ, ইউনিটস এবং ডিভাইস ইত্যাদি।

- MBR = 32 bit, 2^32 x 512 = 2 TB
- GPT = 64 bit, 2^64 x 512 = 9.4 ZB

### ডিস্ক পার্টিশন কি?

আমাদের সিস্টেমের (Windows/Linux) অপারেটিং সিস্টেম এবং ইউজার/এপ্লিকেশন ডেটা আলাদা পার্টিশনে বা ড্রাইভে রাখার জন্য ডিস্ক পার্টিশন করা হয়।

ফলে আমাদের সিস্টেমের কোনো একটি পার্টিশন ক্র্যাশ (Crash) করলে বা পার্টিশন ডিলিট হয়ে গেলেও যাতে অন্য পার্টিশনের ডাটা অক্ষত থাকে।

### পার্টিশন ফরম্যাট বা ফাইল সিস্টেম কি?

অপারেটিং সিস্টেম ভেদে এক একটি পার্টিশনের ফরম্যাট আলাদা আলাদা পদ্ধতিতে হয়ে থাকে। যেমনঃ উইন্ডোজ অপারেটিং সিস্টেমের ক্ষেত্রে `NTFS` এবং `FAT32` পার্টিশন ফরম্যাট ব্যবহার করা হয়ে থাকে।

`NTFS` এর ক্ষেত্রে `Security, encription, Compression, File Size, Block Size, Partition Size` ইত্যাদি ফিচার থাকে, যেটা `FAT32` তে পাওয়া যাবে না। যেমনঃ `FAT32` তে ম্যাক্সিমাম ডাটা ক্যাপাসিটি ৩২ গিগা বাইট, অপরদিকে `NTFS` 2TB পর্যন্ত সাপোর্ট করে, আবার `NTFS` ফাইল সিস্টেম ডাটা `Encryption` সিস্টেম আছে, কিন্ত `FAT32` তে নাই।

- Windows File System: FAT32, NTFS
- Linux File sytem: ext2, ext3, ext4, XFS (current), vfat, swap, GFS2, ZFS (BSD)

যেমনঃ EXT4 ফাইল সিস্টেম 50 TB, এবং XFS ফাইল সিস্টেম 1 PB পর্যন্ত সাপোর্ট করে।

All device files location: /dev/\*\* SSD, HDD, DVD, USB, Serial, Keyboard, Sound Card, Virtual Terminal (tty)

- MBR ==> 512-byte sectors, 2TB Max Partition Size (Single)
- GPT ==> 512-byte sectors, 9.4 Zettabytes Max Partition Size

#### Total Partition: MBR - BIOS (dos)

Linux Partition = 15 (4 Primary + 11 Logical)

#### Total Partition: GPT - UEFI

Total Partition: 128 (Windows/Linux)

### Disk Name:

নতুন হার্ডডিস্ক / হারড্রাইভ এ্যাড করলে নিচের নামের মতো দেখা যাবে।

- পাটা ক্যাবল - hdx [a-z] [aa-az] (অনেক পুরনো)
- সাটা ক্যাবল - sdx [a-z] [aa-az]
- ভার্চুয়াল ডিস্ক - vdx [a-z] [aa-az] [ba-bz]
- Disk for Cloud - xvdx [a-z] [aa-az] (Cross Platform)
- NVMe - nvme0n1, nvme0n2, nvme0n3 এভাবে থাকবে
- CD/DVD - sr0, sr1, sr2 etc.

আর পার্টিশনের ক্ষেত্রে নিচের নামের মতো আসবে। সাটা ক্যাবলের ক্ষেত্রে এমন হবে আর বাকিগুলোর জন্যও ঐ হার্ডডিস্ক অনুযায়ী হবে।

- sda = 1st disk
- sdb = 2nd disk
- sdc = 3rd disk

- IDE/SATA/SAS/SCSI HDD: sda, sdb, sdc
- NVMe = nvme
- Linux Virtual Machine: vda, vdb (Exam)
- USB: sda1, sdb1
- DVD: sr0

### For Linux

| Feature                          | Ext2                        | XFS                  | VFAT                          | Ext3              | Ext4                     | Btrfs              |
| -------------------------------- | --------------------------- | -------------------- | ----------------------------- | ----------------- | ------------------------ | ------------------ |
| Full Form                        | Second Extended File System | Extended File System | Virtual File Allocation Table | 3rd Extended File | 4th Extended File System | B-Tree File System |
| Introduce                        | 1993                        | 1994                 | 1995                          | 2001              | 2008                     | 2009               |
| Individual File Size             | 16BG - 2TB                  | 8EB                  | 4GB                           | 16GB - 2TB        | 16TB                     | 16EB               |
| Volume File System Size          | 4TB - 32TB                  | 8EB                  | 2TB                           | 4TB - 32TB        | 1EB                      | 16EB               |
| Maximum File Name Length         | 255 Bytes                   | 255 Bytes            | 255 Bytes                     | 255 Bytes         | 255 Bytes                | 255 Bytes          |
| Maximum Number of Files          | 2 Billion                   | 2^6                  | 4 Billion                     | 2 Billion         | 4 Billion                | 2^64               |
| Maximum Number of Subdirectories | 32,000                      | Unlimited            | 512                           | 32,000            | 64,000                   | Unlimited          |

### For Windows

| Feature                          | FAT                   | FAT16                          | NTFS                       | FAT32                          | exFAT                            | ReFS                  |
| -------------------------------- | --------------------- | ------------------------------ | -------------------------- | ------------------------------ | -------------------------------- | --------------------- |
| Full Form                        | File Allocation Table | File Allocation Table - 16 Bit | New Technology File System | File Allocation Table - 32 Bit | Extensible File Allocation Table | Resilient File System |
| Introduce                        | 1977                  | 1984                           | 1993                       | 1996                           | 2006                             | 2012                  |
| Individual File Size             | 2GB                   | 2GB                            | 16EB                       | 4GB                            | 4GB                              | 16EB                  |
| Volume File System Size          | 2GB                   | 2GB                            | 256TB                      | 32GB                           | 128PB                            | 256ZB                 |
| Maximum File Name Length         | 11 Bytes              | 255 Bytes                      | 255 Bytes                  | 255 Bytes                      | 255 Bytes                        | 32,768 Bytes          |
| Maximum Number of Files          | 65,536                | 65,536                         | 2^32                       | 65,534                         | 27,96,202                        | 2^64                  |
| Maximum Number of Subdirectories | 512                   | 65,536                         | Unlimited                  | 65,536                         | 2^64                             | Unlimited             |

`fdisk -l` `cfdisk` `parted` কমান্ড দিয়ে `print free` লিখে এন্টার প্রেস করলে আরো কিছু তথ্য পাওয়া যাবে।

নোটঃ `parted` থেকে বের হওয়ার জন্য `q` প্রেস করতে হবে।

### নতুন পার্টিশন তৈরি করা

`fdisk /dev/sda` MBR/GPT based পার্টিশন করার ক্ষেত্রে ব্যবহার হয়। আর শুধুমাত্র `GPT Based` পার্টিশন করার জন্য `gdisk /dev/sda` ব্যবহার করা হয়।

`Command (m for help):` টারমিনালে এমন আসলে `m` বাটন প্রেস করার মাধ্যমে বিভিন্ন হেল্প অপশন পাওয়া যাবে।

- `d` পার্টিশন ডিলিট করার জন্য ব্যবহার হয়।
- `l` কি কি ধরনের পার্টিশন ID আছে সেটা জানার জন্য।
- `m` প্রেস করে বিভিন্ন হেল্প অপশন পাওয়া যাবে।
- `n` নতুন পার্টিশন তৈরি করার জন্য।
- `p` কি কি পার্টিশন তৈরি করা আছে সেটা জানার জন্য।
- `q` হেল্প অপশন থেকে বের হওয়ার জন্য।
- `t` পার্টিশন আইডি পরিবর্তন করার জন্য।
- `w` পার্টিশন তৈরি/আপডেট করার পরে সেভ করার জন্য `w` (Write) প্রেস করতে হবে। এমন আর অপশন আছে যেগুলো প্রয়োজন অনুযায়ী ব্যবহার করতে পারি।

## Partition

এখন নতুন পার্টিশন তৈরি করার জন্য আমরা `n` প্রেস করবো। এর পরে পার্টিশন নাম্বার দিয়ে এন্টার প্রেস করতে হবে। যদি কোন পার্টিশন নাম্বার না দেই তাহলে অটোম্যাটিক্যালি আগের পারটিশন নাম্বার অনুযায়ী পরের পারটিশন নাম্বারটি বসবে। যেমন আগের পারটিশন নাম্বার যদি হয় ৩ তাহলে পরেরটি হবে ৪ নাম্বার পারটিশন।

তারপর ফার্স্ট সেক্টর বলে দিতে হবে। সাধারণত এটা আমরা চেঞ্জ করি না। যেমন আছে তেমন রেখে দেই। এর পরে লাস্ট সেক্টর বলে দিতে হবে। অর্থাৎ পারটিশন সাইজ বলে দিতে হবে। সাইজের ক্ষেত্রে `K,M,G,T,P` এই ফরম্যাটটি ফলো করলেই হবে। +500M এখানে আমি ৫০০ মেগাবাইটের একটা পার্টিশন তৈরি করলাম।

`K for Kilobyte`
`M for Megabyte`
`G for Gigabyte`
`T for Terabyte`
`P for Petabyte`

সাইজ উল্লেখ করার আগে + (Plus) অথবা - (Minus) সাইন দিতে হবে। + (Plus) সাইন ব্যাবহার করলে ফার্স্ট সেক্টর থেকে সে স্পেস নিবে আর - (Minus) সাইন ব্যাবহার করলে লাস্ট সেক্টর থেকে ঐ নির্দিষ্ট পরিমাণ স্পেস রেখে বাকি স্পেসটুকু নিয়ে একটা পার্টিশন তৈরি হবে।

### Partition with dos (MBR)

```
Command (m for help): n
Partition type
   p   primary (2 primary, 0 extended, 2 free)
   e   extended (container for logical partitions)
Select (default p):

Using default response p.
Partition number (3,4, default 3):
First sector (73418752-83886079, default 73418752):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (73418752-83886079, default 83886079): +500M

Created a new partition 3 of type 'Linux' and of size 500 MiB.
```

নোটঃ উপরে আমরা `500M` সাইজের একটি পার্টিশন তৈরি করেছি। এই নতুন পার্টিশনটি দেখার জন্য `p` বাটন প্রেস করতে হবে।

নোটঃ Extended পার্টিশনের ক্ষেত্রে `First Sector` এবং `Last Sector` উভয়ের ক্ষেত্রেই এন্টার প্রেস করতে হবে। তাহলে সম্পূর্ণ ফ্রি স্পেস থেকে একটা প্রাইমারি পার্টিশন তৈরি হয়ে যাবে। যেই প্রাইমারি পার্টিশন থেকে লজিক্যাল পার্টিশন তৈরি করা যাবে। অর্থাৎ, যখন extended পার্টিশন করা হয় তখন দুই বার এন্টার প্রেস করতে হবে।

```
Command (m for help): p


Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xce313077

Device     Boot    Start      End  Sectors  Size Id Type
/dev/sda1  *        2048  2099199  2097152    1G 83 Linux
/dev/sda2        2099200 73418751 71319552   34G 8e Linux LVM
/dev/sda3       73418752 74442751  1024000  500M 83 Linux
```

এখানে `/dev/sda3` নামে একটা `500M` সাইজের একটি পার্টিশন তৈরি হয়েছে। এখন যদি সেইভ করতে চাই তাহলে `w` প্রেস করতে হবে।

### Partition with gpt

```
Command (m for help): n

Partition number (5-128, default 5):

First sector (48646144-62914526, default 48646144):

Last sector, +/-sectors or +/-size{K,M,G,T,P} (48646144-62914526, default 62914526): +500M

Created a new partition 5 of type 'Linux filesystem' and of size 500 MiB.

Command (m for help): p
Disk /dev/sda: 30 GiB, 32212254720 bytes, 62914560 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 838FAD20-7BA1-4672-804E-5C97CB354F7E

Device        Start      End  Sectors  Size Type
/dev/sda1      2048   411647   409600  200M EFI System
/dev/sda2    411648  2508799  2097152    1G Linux filesystem
/dev/sda3   2508800  6703103  4194304    2G Linux swap
/dev/sda4   6703104 48646143 41943040   20G Linux filesystem
/dev/sda5  48646144 49670143  1024000  500M Linux filesystem

Command (m for help):
```

এখানে `/dev/sda5` নামে একটা `500M` সাইজের একটি পার্টিশন তৈরি হয়েছে। এখন যদি সেইভ করতে চাই তাহলে `w` প্রেস করতে হবে।

Block Size বাই ডিফল্ট 256 byte থাকে। কেউ চাইলে এটা কাস্টমাইজ করে নিতে পারে। তবে এক্সপারটিজ ছাড়া কাস্টমাইজ করা উচিত না। Block Size কাস্টম করলে নিচের সাইজ রিকমেন্ডিড।

```
256 byte
512 byte
1024 byte
2048 byte
```

পার্টিশন তৈরি করার পরে `p` প্রেস করে দেখতে পারি পার্টিশন তৈরি হয়েছে কিনা। আবার `w` প্রেস করে সেইভ করেও বের হয়ে আসতে পারি।

`[root@devs ~]# lsblk` কমান্ড দিয়ে আমরা দেখতে পারি পার্টিশন তৈরি করার পরে সিস্টেমের কাছে আপডেট গেছে কিনা। RHEL 7 বা তার আগের ভার্সনে ফার্স্ট আটেম্পের আপডেট যেত কিন্তু সেকেন্ড আটেম্পটের আপডেট যেত না। অর্থাৎ সিস্টেম বুজতে পারতো না যে নতুন একটা পার্টিশন তৈরি করা হয়েছে।

`partprobe` Partition Provisioning যদি কোন কারণে সিস্টেমের কাছে পার্টিশন তৈরির আপডেট না যায় তখন এই কমান্ড রান করতে হয়। তবে এটা খুব সাবধানে রান করতে হয়। কারন একাধিক হার্ডডিস্ক থাকলে তখন সিস্টেম আপ হতে কিছু টাইম নিবে।

`partprobe /dev/sda /dev/sdb` এইভাবে আমরা একাধিক হারডিস্কে বলে দিতে পারি যে কোন কোন হারডিস্ককে আপডেট বা প্রবিশনিং করা দরকার।

`reboot` দিয়েও সিস্টেমের কাছে আপডেট জানাতে পারি।
