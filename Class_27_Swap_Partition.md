# RHCSA Class 27: Swap Partition

`[root@devs ~]# free -mh` মাউন্ট করার আগে আমরা চেক করে নিতে পারি মেমোরি স্ট্যাটাস।

`swapon` মাউন্ট করা

`swapoff` আনমাউন্ট করা

`swapon /dev/sda5`

`swapon -s` দিয়ে আমরা দেখতে পারি কতগুলো এবং কোন কোন পার্টিশন `swap` পার্টিশন হিশেবে আছে।

একটা নতুন পার্টিশন তৈরি করে পার্টিশন টাইপ `swap` করে দিতে হবে। এজন্য `t` প্রেস করে পার্টিশন নাম্বার এর পরে পার্টিশন আইডি অথবা পার্টিশন এলিয়েস দিতে হবে।

### Mount Swap Partition

আমার ক্ষেত্রে `swap partition` হল `sda3`

`[root@devs ~]# mkswap /dev/sda3`

`[root@devs ~]# swapon /dev/sda3` `swap` পার্টিশন মাউন্ট করা।

`[root@devs ~]# free -mh` `swap` পার্টিশন মাউন্ট এবং মারজ হলো কিনা চেক করতে পারি।

Highest value heightest priority

`[root@devs ~]# swapoff /dev/sda3` `swap` পার্টিশন আনমাউন্ট করা। পার্টিশন আনমাউন্ট করার পরে `/etc/fstab` ফাইল থেকে এন্ট্রি রিমুভ করতে হবে এবং পার্টিশন ডিলিট করলেই `swap` পার্টিশন ডিলিট হয়ে যাবে।

### Partition delete

- Remove fstab entry (vim /etc/fstab)
- Unmount
- Then delete

`[root@devs ~]# vim /etc/fstab`

`/etc/fstab` ফাইল ওপেন করার পরে, `/dev/sda3 & /dev/sda4` পার্টিশনের এন্ট্রিগুলো রিমুভ করে দিতে পারি অথবা `#` সাইন দিয়ে কমেন্ট করে দিতে হবে।

`[root@devs ~]# umount /disk1` এখানে পার্টিশন নাম বা ডিরেকটরি নাম বলে দিতে হবে।

`[root@devs ~]# umount /disk2`

`[root@devs ~]# fdisk /dev/sda`

```
Command (m for help): d
Partition number (1-3, default 3):

Partition 3 has been deleted.
```

`Command (m for help): p` `p` প্রেস করে পার্টিশন লিস্ট দেখতে পারি অথবা `w` প্রেস করে সেইভ করে বের হয়ে আসতে পারি।

`Command (m for help): w`

নোটঃ পার্টিশন ডিলিট করার আগে অবশ্যই `/etc/fstab` ফাইল থেকে এন্ট্রিগুলো রিমুভ করতে হবে এবং আনমাউন্ট করতে হবে।

`[root@devs ~]# fdisk -l`

এই একইভাবে আমরা `ISO` ফাইল এবং `USB` ডিভাইস মাউন্ট করতে পারি। তবে এক্ষেত্রে `/etc/fstab` ফাইলে এন্ট্রি দেয়ার দরকার নেই।

`[root@devs ~]# mount -t ntfs /dev/sdb1 /mnt` যদি NTFS ফাইল সিস্টেম হয়।

`mkdir /{software,rhcsa,devops}` একাধিক ডিরেক্টরি একসাথে তৈরি করার কমান্ড।

### Home Work 1

- mount your NTFS based Pendrive under '/mnt' directory.
- For NTFS Pendrive you have to install 'ntfs-3g' application.

`[root@devs ~]# yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm`

`[root@devs ~]# yum install ntfs-3g -y`

[Watch this video](https://www.youtube.com/watch?v=bbbmM6CuEnE)

------------------------ x -----------------------

### Home Work 1

- The node1 machine has unused space. On the unused space, create a 400 MiB GPT
  partition.

- Format the 400 MiB partition with an XFS file system and persistently mount it to the
  /backup directory.

- To verify your work, reboot the Node1 machine. Confirm that the system automatically
  mounts the partition onto the /backup directory.
