# Day 05: Introduction to Command Line Interface (CLI)

`~` এই সাইনকে টিলডা বলে। এই টিলডা সাইন দিয়ে লিনাক্সে `Home Directory` বুঝায়।

## Linux User's Types:

লিনাক্স সিস্টেমে ৩ (তিন) ধরণের ইউজার একাউন্ট থাকে।

- root user: Administrator - `[root@devs ~]#` সিস্টেমের অ্যাডমিন ইউজার। `#` থাকা মানে হলো সে `root` ইউজার।
- system user: service (mail/ftp/ssh/daemons) - cannot login (সিস্টেমে বিল্ট-ইন থাকে) বা কোন সফটওয়্যার ইন্সটল করলে এক বা একাধিক ইউজার তৈরি হতে পারে।
- regular user / non root user: student, hasan, munir ($) - যে সকল ইউজার একাউন্ট `root` বা `sudo` প্রিভিলেজ আছে এমন ইউজার কর্তৃক তৈরি করা হয়।

`[root@devs Desktop] #` এই লাইনটিকে ব্যাখ্যা করা যাক।

- `root` user name (যে ইউজার দিয়ে সিস্টেমে লগইন করা হয়েছে)
- `devs` hostname (কম্পিউটারের/সিস্টেমের নাম)
- `Desktop` user's (ইউজারের বর্তমান অবস্থান)
- `#` user types `# for root`, `$ for regular user`

## Working with Linux Shells & Terminal:

User -> Keyboard -> Terminal/Application -> Shell -> Kernel -> Hardware (CPU/MEM/DISK)
Screen <- Terminal/Application <- Shell <- Kernel <- Hardware

নোটঃ শেল হলো একটা ইন্টারপ্রেটার যেটা টারমিনালের ব্যাকগ্রাউন্ডে কাজ করে। টারমিনাল শুধুমাত্র একটা ইন্টারফেইস। কোন কম্যান্ড শেলে না থাকলে সেটা কাজ করবে না। আমাদের শেলের নাম হলো ব্যাস।

## Linux Shell পরিচিতিঃ

লিনাক্স অপারেটিং সিস্টেমের একটি গুরুত্বপূর্ণ ও স্পেশাল প্রোগ্রাম হচ্ছে লিনাক্স শেল। লিনাক্সের শেল ইউজারের কাছ থেকে কমান্ড গ্রহণ করে এবং যদি ইউজার কর্তৃক প্রদত্ত কমান্ড সঠিক হয়, তাহলে সিস্টেমের শেল এই কমান্ডকে কার্নেলের নিকট প্রেরণ করে অপারেটিং সিস্টেমের সঙ্গে তথ্য আদান-প্রদান করার জন্য লিনাক্স ইউজার ইন্টারফেস প্রদান করে। আবার প্রদত্ত কমান্ড যদি সঠিক না হয়, তাহলে `Bash: Command not Found` দেখাবে।

লিনাক্স শেল হচ্ছে একটি কমান্ড ল্যাঙ্গুয়েজ ইন্টারপ্রেটার, যেটা কিবোর্ড অথবা ফাইল হতে কমান্ড গ্রহণ করে এবং যেকোনো Programming Syntax বা কমান্ডকে সরাসরি execute করতে পারে।

### Types of Linux Shell:

- sh
- Bash (commonly used)
- cshell
- tcshell
- kshell
- zsh

বর্তমানে সিস্টেমে কোন শেল ব্যবহার হচ্ছে এবং আমাদের সিস্টেমে এবং কি কি শেল আছে সেটা জানার জন্য নিচের কমান্ডঃ

`echo $SHELL`

`chsh -l`

আউটপুটঃ

```
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
```

## Linux Consoles:

- Virtual (Physical) Consoles:

  - Ctrl + Alt + F1/F2: GUI
  - Ctrl + Alt + F3-F6: CMD (tty)

- Pseudo Consoles (/dev/pts/x) - যে সকল টার্মিনাল আমরা গ্রাফিক্যালি রান করি, সেগুলাকে বলা হয়, `Pseduo` কনসোল।
- Virtual (Remote) Console (Telnet, SSH, RDP, VNC) - যে সকল টার্মিনাল আমরা রিমোট থেকে লগিন করি।
- Web Console using https (cockpit) - ওয়েব ব্রাউজার দিয়ে লগিন করলে সেটাকে ওয়েব কনসোল বলে।

## Classic Desktop এর ক্ষেত্রেঃ

Alt + Ctrl + F1 => GUI - seat0
Alt + Ctrl + F3 => new CMD terminal (F3-F6) - tty3-tty6

নোটঃ ফিজিক্যাল কনসোল দিয়ে লগইন করলে এবং টার্মিনাল থেকে `who` কমান্ড দিলে সেখানে `tty` দেখা যাবে। উদাহরণ স্বরূপ কেউ যদি, `Alt + Ctl + F3` দিয়ে লগইন করে, তাহলে `who` কমান্ড দিলে `tty3`। কিছু কিছু ক্ষেত্রে গ্রাফিক্যাল টার্মিনালের ক্ষেত্রে `:0` দেখাবে।

`Press Ctl + Alt + F3`

- `su -` For Fedora Distribution রেগুলার ইউজার থেকে `root` ইউজারে সুইচ করার কম্যান্ড।
- `su -i` For Debian Distribution রেগুলার ইউজার থেকে `root` ইউজারে সুইচ করার কম্যান্ড।
- `su - student` রূট `root` ইউজার থেকে রেগুলার ইউজারে সুইচ করার কম্যান্ড।

`su for switch user`

`exit` কম্যান্ডের মাধ্যমে তার আগের স্টেজে বা আগের ইউজারে ফিরে আসতে পারি বা লগআউট হয়ে যেতে পারি।

## Enable Web Console (Cockpit):

`systemctl enable --now cockpit.socket` (Root Authentication Required)

- Open Firefox web browser and run https://localhost:9090
- Login by root username and password

### লিনাক্সে বহুল ব্যবহৃত কিছু শর্টকাট কমান্ডঃ

- `ctrl + l` - টার্মিনাল পরিষ্কার (clear) করার জন্য।
- `ctrl + shift + t` - গ্রাফিক্যাল ইউজার ইন্টারফেস দিয়ে (GUI) নতুন ট্যাব খোলার জন্য।
- `Ctrl + d` - কমান্ড কনসোলে লগ আউট করার জন্য ব্যবহার হয়।
- `Alt + F2` - নতুন কমান্ড রান করার জন্য। `Type 'firefox' and press Enter`

## Linux Command Syntax/Pattern:

প্রতিটি কমান্ড লাইন ইন্টারফেস `CLI` ভিত্তিক অপারেটিং সিস্টেম বা ডিভাইসে কমান্ড লাইন ইন্টারফেস নিয়ে কাজ করতে চাইলে, প্রত্যেকটি কমান্ডের ০৩ (তিন) টা পার্ট থাকে। প্রথম টা হচ্ছে কমান্ড, যেটা দিতেই হবে এবং শেষের টা হচ্ছে আর্গুমেন্ট।

কিছু কিছু কমান্ড আর্গুমেন্ট ছাড়াই কাজ করে আবার কিছু কিছু কমান্ড আর্গুমেন্ট ছাড়া কাজ করবেই না। আবার অনেক কমান্ডে আমরা কমান্ডের সাথে অপশন ব্যবহার করে থাকি। একটা কমান্ডের সাথে নানা রকম অপশন ব্যবহার করা যায় এবং একটি একটি অপশন ব্যবহার করে আলাদা আলাদা আউটপুট পাওয়া যায়।

নিচের লাইনে আমরা একটা কমান্ডের তিন টা পার্ট দেখতে পাচ্ছি।

`command [options (-)] argument`

- date
- cal
- ping
- ping 127.0.0.1
- ping -c 4 127.0.0.1

`ping --help`

উপরে দেখতে পাচ্ছি, একটি কমান্ডের কমান্ড, অপশন এবং আর্গুমেন্ট `Command, Option, Argument` ইত্যাদি সবই ব্যবহার করা হয়েছে। এখানে `ping` হচ্ছে একটি কমান্ড, `-c4` হচ্ছে অপশন এবং `www.google.com` হচ্ছে আর্গুমেন্ট। এই কমান্ডে শুধু `ping` লিখে এন্টার চাপলে কাজ করবে না। কারণ, `ping` কোন `Host/Node` কে করবো? এইজন্য অবশ্যই আর্গুমেন্ট ব্যবহার করতে হবে।

একটা কমান্ডের সাথে অপশন ব্যবহার করে কিভাবে বিভিন্ন আউটপুট পাওয়া যায়, তার একটা উদাহরণ নিচে দেওয়া হয়েছে। এখানে `ls` কমান্ডের সাথে `-l,-i,-a,-h` নানা অপশন ব্যবহার করে নানা ধরণের আউটপুট পাওয়া যাবে। পাশাপাশি আমরা চাইলে একসাথে সবগুলা অপশন ব্যবহার করতে পারি।

- `ls` বর্তমান ডিরেক্টরির মধ্যে সকল ফাইল/ডিরেক্টরি দেখা যাবে।
- `ls -l` লং লিস্ট আকারে `Properties` দেখা যাবে।
- `ls -li` ফাইল/ডিরেক্টরির আইনোড `inode` সহ দেখা যাবে।
- `ls -la` লুকানো `Hidden` ফাইল সহ দেখা যাবে।
- `ls -lh` `Human Readable` (K/M/G) তে দেখা যাবে।
- `ls -laih` সকল অপশন একসাথে ব্যবহার করা হয়েছে।

উপরে `ls` কমান্ডের সাথে ব্যবহৃত অপশন সমূহের বিস্তারিত নিচে আলাপ করা হয়েছেঃ

- `-l = list` (লম্বা লিস্ট বা বিস্তারিত দেখার জন্য এই অপশন টা ব্যবহার করা হয়)
- `-i = inode` (প্রতিটি ফাইল/ডিরেক্টরির একটা ইন্ডেক্সিং নাম্বার থাকে যেটাকে বলা হয়ে আইনোড `inode`, এটা মূলত সব ফাইলের আলাদা আলাদা।)
- `-a = all` এই অপশন ব্যবহার করলে বর্তমান ডিরেক্টরির সকল লুকায়িত `Hidden` ফাইল/ডিরেক্টরিসহ দেখা যাবে।
- `-h = Human Readble` ফাইলের সাইজ সাধারনত বাইট আকারে দেখায়। এই অপশন ব্যবহার করলে `Kilo (K), Mega (M), Giga (G)` আকারে দেখাবে।

আর যদি সবগুলা অপশন একসাথে ব্যবহার করা হয়, তাহলে সবগুলা অপশনের আউটপুট একসাথে দেখাবে।

`pwd` বর্তমানে কোন ডিরেক্টরির মধ্যে অবস্থান করছি, সেই আউটপুট দেখাবে।

- `~` Home Directory (root/regular user)
- `/` root partition (/) (Computer)
- `/root` রুট `root` ইউজারের `~` Home Directory
- `/home` সকল রেগুলার ইউজারদের `~` Home Directoryi.e.: /home/student

`[student@devs ~]$` রেগুলার ইউজার `student` এর `~` Home Directory

## কিছু বেসিক কমান্ড যেগুলা লিনাক্সের সকল ডিস্ট্রিবিউশনের মধ্যে কাজ করবেঃ

- `cal`
- `cal 2024`
- `date`
- `who` who are currently loggin (shorter)
- `w` who are currently loggin (Details)
- `tty`
- `hostname`
- `uname -r` kernel version
- `uname` OS name
- `cat /etc/redhat-release` redhat/centos version
- `top` Current process status like windows task manager (বের হওয়ার জন্য `q` প্রেস করতে হবে)
- `lastlog` login details
- `free -m` RAM Info
- `uptime` system UPtiem info
- `ifconfig` IP Address Info
- `history` list of privious command
- `!30` history থেকে `30` নাম্বার কমান্ডটি পুনরায় রান করার জন্য।
- `history -c` history থেকে সকল কমান্ড মুছে দেওয়ার জন্য ব্যবহ্রত হয়।
- `reboot or init 6` সিস্টেম রিবুট / রিস্টার্ট
- `poweroff or init 0` সিস্টেম শাটডাউন

## Linux Directory Structure:

- `cd /` রুট `root` পার্টিশনে (/) প্রবেশ করা হল।
- `ls` রুট `root` পার্টিশনের (/) মধ্যে কি কি আছে সেটা দেখা যাবে।

নিচে ডিরেক্টরিগুলোর নাম দেয়া হলো।

```
boot etc lib media opt root sbin sys usr
bin dev home lib64 mnt proc run srv tmp var
```

- `/bin - Binary Files` যে সকল কমান্ড গুলা রেগুলার ইউজার কর্তৃক রান হয়, যেমনঃ `ls, cd, pwd, who, cal, date` ইত্যাদি।

- `/boot` - সিস্টেম বুট `boot` সম্পর্কিত ফাইল। যেমনঃ `boot loader, Kernel, itramfilesystem` এখানে থাকবে।

- `/dev` - সিস্টেমের সকল ডিভাইস টাইপের ফাইল গুলো এখানে থাকে। যেমনঃ `dvd, HDD, Printer, sound, Terminal, Serial/Prallel Devices` ইত্যাদি।

- `/etc` - সকল সিস্টেম এবং সার্ভার কনফিগারেশন ফাইল। ইউজার/গ্রুপ ডাটাবেজ, সিস্টেম ও সার্ভার কনফিগারেশন, প্রোফাইল, সিকিউরিটি সম্পর্কিত ফাইল।

- `/home` - সকল রেগুলার ইউজারদের হোম ডিরেক্টরি। যেমনঃ `student, munir, shakil` (FTP, Email, File সার্ভার ইউজার)।

- `/lib` - সিস্টেমের সকল `library` ফাইল এবং কার্নেল মডিউল থাকে `/lib` এর ভিতরে। `/bin & /sbin` ডিরেক্টরিতে যত কমান্ড আছে, এই কমান্ড এক্সিকিউট করতে `library` দরকার হয়।

- `/media` - এটা ডিফল্ট ভাবে ফাঁকা থাকে। মিডিয়া টাইপের ডিভাইস `DVD/ISO` বা এক্সটারনাল ডিভাইস এই লোকেশনে মাউন্ট করে এক্সেস করা হয়।

- `/mnt` - mount point (DVD/HDD/USB/Network Share) - ইত্যাদি এই লোকেশন মাউন্ট করে এক্সেস করা যায়।

- `/opt` - optional (empty) - এই ডিরেক্টরি সাধারণত ফাঁকা থাকে। বিভিন্ন ফাইল/আপ্লিকেশন/সার্ভিস টেস্ট করার জন্য এই ডিরেক্টরি কে ব্যবহার করা হয়।

- `/proc` - Also called 'proFS' system process related info (CPU, RAM, Process, Running Apps, Driver, Modules and Kernel)

- `/root` - লিনাক্সের অ্যাডমিন ইউজার `root` রুট এর হোম ডিরেক্টরি। সাধারণ ইউজাররা `student, munir, shakil` এখানে প্রবেশ করতে পারে না।

- `/run` - service running data. Runtime data for processes started since the last boot.

- `/sbin` - System Binary Files যে সকল কমান্ড গুলা root ইউজার কতৃক রান করা হয়, যেমনঃ `useradd, groupadd, userdel, fdisk` এই কমান্ড সমূহ `/sbin` ডিরেক্টরিতে পাওয়া যাবে।

- `/srv` for Service. User's (/home/\*) service related data. Like WWW, FTP, Email etc.

- `/sys` for system. `/sys` directory as a virtual filesystem (sysfs) mounted under `/sys`. similar as proc.

- `/tmp` - Temporary ফাইল সমূহ এখানে থাকে। সিস্টেম প্রতি ১০ দিন পর পর নিজে থেকেই মুছে `Delete` ফেলবে।

- `/usr` - Thirdparty software install location + সিস্টেমের যত কমান্ড থাকে `/usr/bin, /usr/sbin` + Libary File

- `/var` - variable file (mail, log,hosting,ftpdata,dynamic data)

- `cd` লিখে এন্টার প্রেস করলে আমরা যে ডিরেক্টরিতেই থাকি না কেন, সরাসরি হোম ডিরেক্টোরিতে নিয়ে আসবে।

- `cd /` `cd` কমান্ডের সাথে `/` ব্যবহার করে এন্টার প্রেস করলে রুট পার্টিশনে (/) প্রবেশ করা যাবে।

- cd /dir1/dir2/dir3 Linux Directory Path Forward Slash (/) ব্যবহার করা হয়।

- C:\dir1\dir2\dir3> Windows Directory Path Back Slash (\) ব্যবহার করা হয়।

`cd ..` এক ধাপ পিছনের `Parent` ডিরেক্টরিতে যাওয়ার কমান্ড।

`cd -` পুর্বের ডিরেক্টরিতে ফিরে যাওয়ার জন্য। অর্থাৎ, বর্তমানে যেই ডিরেক্টরিতে আছি ঠিক তার আগে যেই ডিরেক্টরিতে ছিলাম সেই ডিরেক্টরিতে নিয়ে যাবে।

`cd Videos` Videos ডিরেক্টরি থেকে Music ডিরেক্টরিতে যেতে চাইলে `cd ../Music`

- `touch file1` ফাইল তৈরির জন্য `touch` কমান্ড ব্যবহার করা হয়।
- `ls` বর্তমান ডিরেক্টরিতে কি কি ফাইল এবং ডিরেক্টরি আছে তার লিস্ট দেখা যায়।
- `touch file2 file3 file4` একাধিক ফাইল একসঙ্গে তৈরি করা যায়।
- `mkdir dir1 dir2 dir3` এক সাথে একাধিক ডিরেক্টরি তৈরি করা যায়।
- `mkdir -p dir1/linux/redhat` ডিরেক্টরি পাথ তৈরির কমান্ড। `-p for parent`
- `touch dir1/linux/redhat/file` ডিরেক্টরি পাথে ফাইল তৈরির কমান্ড।
- `cd dir1/linux/` ডিরেক্টরি পাথে প্রবেশ করার জন্য।
- `pwd` আমার বর্তমান অবস্থান।
- `cd /home/student/dir1/linux` রুট `root` পার্টিশন (/) হয়ে 'linux' ডিরেক্টরিতে প্রবেশ করার জন্য।
- `cd ../..` দুই ধাপ পিছনে (Parent) ডিরেক্টরিতে যাওয়ার কমান্ড।

## Working with File/Directory Properties:

যখন উইন্ডোজ অপারেটিং সিস্টেমে কোনো ফাইল/ডিরেক্টরি সম্পর্কে বিস্তারিত জানার দরকার হয়, তখন ডিরেক্টরির উপরে মাউসের ডান বাটন ক্লিক করে `Properties` থেকে বিস্তারিত তথ্য পাওয়া যায়। যেমনঃ সিকিউরিটি, সাইজ, টাইপ, মোডিফাই/আপডেট তারিখ, ভার্শন ইত্যাদি।

`ll` কমান্ড ব্যবহার করে ফাইল/ডিরেক্টরির বিস্তারিত তথ্য `Properties` দেখা যাবে।

```
d   rwx  rwx  r-x  .   2   student student   8 	Jun 4 06:03 	dir1
-   rw-  rw-  r--  .   1   student student   8 	Jun 4 06:03 	file1

1   2a   2b   2c  2d   3      4      5       6 	    7	          8

1: file/dir types (লিনাক্সে মোট সাত ধরনের (-, d, l, c, b, s, p) ফাইল/ডিরেক্টরি পাওয়া যাবে)
2: file/dir পার্মিশন ফিল্ড এবং এখানে ডট (.) সহ মোট ১০টি ক্যারেক্টার আছে। এই দশটি ক্যারেক্টার কে আবার ৪ টি আলাদা ফিল্ডে ভাগ করা হয়েছে
2a: user permission,
2b: group permission,
2c: others permission
2d: ACL Permission (.)

3: file/dir link (Hard Link) - ফাইলের ক্ষেত্রে '1' আর ডিরেক্টরির ক্ষেত্রে '2' থাকবে, তবে ভিতরে যত ডিরেক্টরি থাকবে তার উপরে ভিত্তি করে সংখ্যা বাড়বে।
4: file/dir owner (যে ইউজার ফাইল/ডিরেক্টরিটি তৈরি করেছে তার নাম থাকবে)
5: file/dir group owner - (ইউজার যে গ্রুপের সদস্য সেই গ্রুপের নাম থাকবে)
6: file/dir size (byte) - ফাইল হলে '0' বাইট এবং ডিরেক্টরি হলে 6 বাইট (XFS) ফাইল সিস্টেমে।
7: file/dir modify date - তৈরির বা আপডেটের তারিখ এবং সময়।
8: file/dir name        - ফাইল বা ডিরেক্টরির নাম।
```

## FIle/dir Permission:

```
r = read
w = write
x = execute
```

- `-` (ড্যাস) থাকা মানে হলো কোন পারমিশন নাই। অর্থাৎ, no permission
- `.` (ডট) মানে হল কোন ACL সেট করা নাই আর `+` প্লাস থাকা মানে হলো ACL সেট করা আছে। `ACL - Access Control List`

## Linux File & Directory types:

- `-` Regular file যেমন টেক্সট ফাইল
- `d` Directory (ফোল্ডার)
- `l` Link file `ls -l /dev/stdin` - (softlink/symbolik link)
- `b` Device/Block Devices `CD/DVD/ISO/HDD/USB` - ls -l /dev/sda ব্লক আকারে ড্যাটা ট্রান্সফার করে তাই একে ব্লক ডিভাইস বলে যেমন 4k, 8k ইত্যাদি। মিনিমাম ব্লক হচ্ছে 1k আর ডিফাল্ট ব্লক হচ্ছে 4k বা কিলোবাইট। ব্লক ডিভাইস বাইট আকারে ড্যাটা ট্রান্সফার করে।
- `c` Character device - `printer, sound card, virtual terminal, serial port, parallel port` - `ls -l /dev/tty` সিঙ্গেল সিঙ্গেল ক্যারেক্টারে ড্যাটা ট্রান্সফার করে, তাই একে ক্যারেক্টার ডিভাইস বলে। ক্যারেক্টার ডিভাইস বিট আকারে ড্যাটা ট্র্যান্সফার করে। এটি এখন এটির ব্যবহার নেই বললেই চলে।
- `s` Socket - `ls -l /run/rpcbind.sock` - A socket file is used to pass information between applications/process for communication purpose
- `p` Pipe file `ls -l /run/initctl` - Pipes are unnamed objects created to allow two processes to communicate. One process reads and the other process writes to the pipe file. This unique type of file is also called a first-in-first-out (FIFO) file.

## Copy, Paste, Remove, Rename, Delete

`cp file1 file2` ফাইল কপি করার জন্য `cp` কমান্ড ব্যবহার করে `file1` টি কপি করে `file2` নামে আরেকটি করা হয়েছে।

- `cp file1 /home/student` ফাইল কপি করে অন্য লোকেশনে `/home/student` একই নামে।
- `cp file1 /home/student/file3` ফাইল কপি করে অন্য লোকেশনে `/home/student` অন্য নামে।
- `cp /etc/passwd .` অন্য লোকেশন থেকে ফাইল কপি করে বর্তমান ডিরেক্টরির (.) মধ্যে নিয়ে আসলাম।
- `cp /etc/hostname /home/student` বর্তমান ডিরেক্টরিতে থেকে অন্য ডিরেক্টরির ফাইল কপি করে অন্য লোকেশনে পাঠানোর যায়।
- `cp -r dir1 backup_dir` ডিরেক্টরি কপি করার জন্য অবশ্যই `cp -r` ব্যবহার করতে হবে। `-r for recursively`
- `cp -r /etc .` `/etc` ফুল ডিরেক্টরি কপি করে বর্তমান ডিরেক্টরির (.) মধ্যে নিয়ে আসার কমান্ড।
- `mv file2 file4` `mv` কমান্ড দিয়ে ফাইল `file2` কে `file4` নামে পরিবর্তন করা হয়েছে।
- `mv file4 /home/student` ফাইল মুভ `cut` করে অন্য লোকেশনে নিয়ে যাওয়া।
- `rm file1` ফাইল মুছে ফেলার জন্য `rm` কমান্ড।
- `rm -r backup_dir` ডিরেক্টরি রিমুভ করার জন্য অবশ্যই `-r` ব্যবহার করতে হবে। `-r for recursively`
- `rm -r backup_dir` সম্পূর্ণ ডিরেক্টরি রিমুভ করার জন্য প্রতিবার `y` প্রেস করে কনফার্ম করতে হবে।
- `rm -rf etc` প্রতিবার `y` প্রেস করা ছাড়াই ফাইল/ডিরেক্টরি মুছা যাবে। `-f for forcefully, r for recursively`
- `rm -rf _` বর্তমান ডিরেক্টরির সকল ফাইল/ডিরেক্টরি কনটেন্ট রিমুভ করার জন্য `_` Underscore ব্যবহার করা হয়।
- `rm -rf * .*` লুকানো ফাইল `Hidden` সহ ডিলিট করে ফেলবে।

`Ctrl + C` প্রেস করে, ফাইল রিমুভ হওয়া থেকে বিরত রাখতে পারি।
