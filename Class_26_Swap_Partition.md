# RHCSA Class 26: File System & Partition Management

`fdisk /dev/sda -l` নির্দিষ্ট কোন হার্ডডিস্কের পারটিশন লিস্ট দেখার যায়।

`blkid` এই কমান্ডের মাধ্যমে আমরা চেক করতে পারি যে ঐ পার্টিশনে কোন ফাইল সিস্টেম দেয়া আছে কিনা। ফাইল সিস্টেম / ফরম্যাট দেয়ার আগে শুধু PARTUUID তৈরি হয় আর ফরম্যাট করার পরে UUID তৈরি হয়।

পার্টিশন তৈরি করার পরে ঐ পারটিশনকে ফরম্যাট / ফাইল সিস্টেম দিতে হবে। পার্টিশন ফরম্যাট না করে আমরা ঐ পার্টিশনে কোন কিছুই রাখতে পারবো না।

`mkfs -t xfs /dev/sda3`

`mkfs -t ext4 /dev/sda4`

অথবা

`mkfs.xfs  /dev/sda3`

`mkfs.ext4  /dev/sda4`

### Drive or Partion Mounting

ফরম্যাট করার পরে পার্টিশন মাউণ্ট করতে হবে। যে ডিরেক্টরির আন্ডারে মাউন্ট করা হবে, আগে সেই ডিরেক্টরি তৈরি করে নিতে হবে।

`mkdir /disk1`

`mkdir /disk2`

রুট পার্টিশনে আমরা `/disk1` এবং `/disk2` নামে ২ টা ডিরেক্টরি তৈরি করে নিলাম।

`mount /dev/sda3 /disk1`

`mount /dev/sda4 /disk2`

`lsblk -f`

নোটঃ এই অবস্থায় সিস্টেম রিবুট দেওয়ার পরে মাউন্ট পয়েন্ট চলে যাবে। কারণ, এটা একটা `Temporary Mount`। Rboot দেয়ার পরে যাতে মাউন্ট পয়েন্ট রিমুভ হয়ে না যায় সেই জন্য আমাদেরকে পারমানান্ট মাউন্ট করে নিতে হবে।

`vim /etc/fstab`

`UUID=66f9ccb5-f0c6-4d33-a61d-59736bafdb13 /disk1                   xfs     defaults        0 0`

অথবা

`/dev/sda3   			/disk1    xfs      defaults     0  0`

`/dev/sda4   			/disk2    ext4      defaults     0  0`

| 1         | 2      | 3   | 4        | 5   | 6   |
| --------- | ------ | --- | -------- | --- | --- |
| /dev/sda3 | /disk1 | xfs | defaults | 0   | 0   |

- 1 - পার্টিশন নাম `/dev/sda3` বা পার্টিশন Block ID অথবা `PARTUUID`. Block ID দেখার জন্য `blkid` কমান্ড রান করতে পারি।
- 2 - মাউন্ট পয়েন্ট (পার্টিশন যে ডিরেক্টরির সাথে ম্যাপিং করা হবে)।
- 3 - ফাইল সিস্টেম টাইপ `xfs, ext4, swap, nfs`
- 4 - Defaults এর সাথে Options হিসেবে `realtime,  noatime, uid, gid, ro, rw, quota, acl, luks, _netdev, ntlmssp(), pri(swap)` (এনক্রিপশন) ব্যবহার করা হয়।
- 5 - ডাম্প প্রোগামের জন্য।
- 6 - ফাইল সিস্টেম চেক অপশন।

নোটঃ ৫ নাম্বার ফিল্ডটি ব্যবহার করার হয় `Dump Program` এর জন্য। যদি ভ্যালু `1` সেট করা হয়, তাহলে Dump ইউটিলিটি পার্টিশন ব্যাকাপের জন্য কাজ করবে। আর যদি ভ্যালু `0` সেট করা হয়, তাহলে Dump ইউটিলিটি পার্টিশন ব্যাকাপের জন্য কোনো কাজ করবে না।

Backup operation – the fifth field contains a 1 if the dump utility should back up a partition or a 0 if it shouldn’t. If you never use the dump backup program, you can ignore this option.

নোটঃ ৬ নাম্বার ফিল্ডটি ব্যবহার করার হয় `fsck` (File System Checking) প্রোগামের জন্য। সিস্টেম বুটিং এর সময় ডিভাইস / পার্টিশনের Error চেক করে। এখানে `0` উল্লেখ করলে `fsck` কাজ করবে না। আর যদি রুট পার্টিশনের ক্ষেত্রে `1` এবং অন্য পার্টিশনের ক্ষেত্রে `2` সেট করা হয়, তাহলে সিস্টেম বুটিং এর সময় ফাইল সিস্টেম চেক করবে।

File system check order – the sixth field specifies the order in which fsck checks the device/partition for errors at boot time. A 0 means that fsck should not check a file system. Higher numbers represent the check order. The root partition should have a value of 1 , and all others that need to be checked should have a value of 2.

`mount -a` সিনট্যাক্স চেক এবং আপডেট করার জন্য।

`mount /dev/sda1` অথবা `mount /software` ডিরেক্টরি অথবা পার্টিশন নাম দিয়ে মাউন্ট করা যায়।

নোটঃ সবচেয়ে ভাল হয় আগে `/etc/fstab` ফাইলে এন্ট্রি দিয়ে এরপরে `mount -a` কমান্ড রান করা। তাহলে সে `fstab` রিড করে নতুন এন্ট্রি অনুযায়ী মাউন্ট করে নিবে।

`mount`

নোটঃ এখন সিস্টেমকে রিবুট দিয়ে চেক করতে পারি যে Parmanently mounting হলো কিনা।

### Swap Partition

`free -mh` মাউন্ট করার আগে আমরা চেক করে নিতে পারি মেমোরি স্ট্যাটাস।

`swapon` মাউন্ট করা

`swapoff` আনমাউন্ট করা

`swapon /dev/sda5`

`swapon -s` দিয়ে আমরা দেখতে পারি কতগুলো এবং কোন কোন পার্টিশন `swap` পার্টিশন হিশেবে আছে।

একটা নতুন পার্টিশন তৈরি করে পার্টিশন টাইপ `swap` করে দিতে হবে। এজন্য `t` প্রেস করে পার্টিশন নাম্বার এর পরে পার্টিশন আইডি অথবা পার্টিশন এলিয়েস দিতে হবে।

### Mount Swap Partition

আমার ক্ষেত্রে `swap partition` হল `sda3`

`mkswap /dev/sda3`

`swapon /dev/sda3` `swap` পার্টিশন মাউন্ট করা।

`free -mh` `swap` পার্টিশন মাউন্ট এবং মারজ হলো কিনা চেক করতে পারি।

Highest value heightest priority

Swap পার্টিশনে প্রায়োরিটি সেট করতে পারি তাহলে সে ঐ পার্টিশনে আগে ড্যাটা রাখবে। `/etc/fstab` ফাইলে গিয়ে `defaults,pri=10` সেট করে দিলেই ঐ পার্টিশনের প্রায়োরিটি হাই হয়ে যাবে।

```
swapoff /dev/sda3
swapon -a
swapon -s
```

`df -h` কতটুকু ডিস্ক ফ্রি আছে তা দেখার জন্য। ফাইল সিস্টেম সহ দেখার জন্যা `T` অ্যাড করে দিতে পারি।

`df -hT`

```
d for Disk
f for free
-h for human
T for type of file system
```

নোটঃ যদি কখনো `swap` পার্টিশনে বেশি স্পেস দরকার হয় তাহলে কখনোই `/` রুট পার্টিশন থেকে স্পেস নেয়া যাবে না।

`swap` পার্টিশনে এক্সট্রা স্পেস অ্যাড করার জন্য।

```
fallocate -l 2G /rhcsa/swap.txt
mkswap /rhcsa/swap.txt
chmod 0600 /rhcsa/swap.txt
/rhcsa/swap.txt swap swap defaults,pri=50  0  0
swapon /rhcsa/swap.txt
free -mh
swapon -s
```

এই ফাইলকে `swap` পার্টিশনে মাউন্টে ইউজ করতে হবে। পার্টিশনে প্রায়োরিটি সেট না হলে সিস্টেমকে রিবুট দিতে হবে।

`-l for length`

`swapoff /dev/sda3` `swap` পার্টিশন আনমাউন্ট করা। পার্টিশন আনমাউন্ট করার পরে `/etc/fstab` ফাইল থেকে এন্ট্রি রিমুভ করতে হবে এবং পার্টিশন ডিলিট করলেই `swap` পার্টিশন ডিলিট হয়ে যাবে।

`gpt` তে ৬৪ টেরাবাইট সাপোর্ট করে আর টোটাল পার্টিশন ১২৮ টা।

### Partition delete

- Remove fstab entry (vim /etc/fstab)
- Unmount
- Then delete

`vim /etc/fstab`

`/etc/fstab` ফাইল ওপেন করার পরে, `/dev/sda3 & /dev/sda4` পার্টিশনের এন্ট্রিগুলো রিমুভ করে দিতে পারি অথবা `#` সাইন দিয়ে কমেন্ট করে দিতে হবে।

`umount /disk1` এখানে পার্টিশন নাম বা ডিরেকটরি নাম বলে দিতে হবে।

`umount /disk2`

`fdisk /dev/sda`

```
Command (m for help): d
Partition number (1-3, default 3):

Partition 3 has been deleted.
```

`Command (m for help): p` `p` প্রেস করে পার্টিশন লিস্ট দেখতে পারি অথবা `w` প্রেস করে সেইভ করে বের হয়ে আসতে পারি।

`Command (m for help): w`

নোটঃ পার্টিশন ডিলিট করার আগে অবশ্যই `/etc/fstab` ফাইল থেকে এন্ট্রিগুলো রিমুভ করতে হবে এবং আনমাউন্ট করতে হবে।

`fdisk -l`

এই একইভাবে আমরা `ISO` ফাইল এবং `USB` ডিভাইস মাউন্ট করতে পারি। তবে এক্ষেত্রে `/etc/fstab` ফাইলে এন্ট্রি দেয়ার দরকার নেই।

`mount -t ntfs /dev/sdb1 /mnt` যদি NTFS ফাইল সিস্টেম হয়।

`mkdir /{software,rhcsa,devops}` একাধিক ডিরেক্টরি একসাথে তৈরি করার কমান্ড।

<!-- Class 27 -->

`MBR` এ প্রাইমারি পার্টিশন ৪ টা করা যায়। চতুর্থ নাম্বার পার্টিশন কে এক্সটেন্ডিড পার্টিশন ও বলে। কারন ঐ পারটিশনকে এক্সটেন্ড করে আর ৫৬ টা লজিক্যাল পারিটিশন করা যায়।

`MBR/DOS` এ ম্যাক্সিমাম পার্টিশন ক্যাপাসিটি ২ টেরাবাইট। যার কারণে `MBR/DOS` এ মোট স্টোরেজ ক্যাপাসিটি (2 T \* 59 = 118) টেরাবাইট।

নোটঃ RHEL 7 বা তার আগের ভার্সনগুলোতে প্রথমবার পার্টিশন তৈরি করার পরে সেইভ দিলে অপারেটিং সিস্টেম বুজতে পারে কিন্তু এর পরে যতবার পার্টিশন তৈরি করা হবে তার আপডেট আর অপারেটিং সিস্টেম এর কাছে যায় না।

`blkid | grep sda`

`xfs` ফাইল সিস্টেম মেইনলি OS এর জন্য ইউজ হয় বা এমন পার্টিশন যেখানে কখনোই স্টোরেজ কমানোর প্রয়োজন পরবে না।

### Home Work 1

- mount your NTFS based Pendrive under '/mnt' directory.
- For NTFS Pendrive you have to install 'ntfs-3g' application.

`yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm`

`yum install ntfs-3g -y`

[Watch this video](https://www.youtube.com/watch?v=bbbmM6CuEnE)

------------------------ x -----------------------

### Home Work 1

- The node1 machine has unused space. On the unused space, create a 400 MiB GPT
  partition.

- Format the 400 MiB partition with an XFS file system and persistently mount it to the
  /backup directory.

- To verify your work, reboot the Node1 machine. Confirm that the system automatically
  mounts the partition onto the /backup directory.
