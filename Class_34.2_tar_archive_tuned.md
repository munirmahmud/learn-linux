# RHCSA Class 34.2: Working with Backup, Archive, Compression and Log Files, Tuned

## Linux Backup, Archive & Compression Utilities:

### Why Need Archive & Compression:

- ১) ফাইল/ডিরেক্টরির সাইজ ছোট করার জন্য। `Compression`
- ২) অনেকগুলা ফাইল/ডিরেক্টরিকে একত্রে বা একটি ফাইলে রুপান্তর করার জন্য।
- ৩) ফাইল সিকিউরিটি দেওয়ার জন্য `Password, Malware attack, missing`
- ৪) ডেটা ব্যাকআপ `Archive` বা অনেক ডেটা এক জায়গা থেকে অন্য জায়গায় পাঠানোর জন্য।
- ৫) ডেটা এনক্রিপশন এবং পাসওয়ার্ড প্রটেক্টেড করার জন্য।
- ৬) ইমেইল এ্যাটাচমেন্ট হিসেবে ব্যবহার করার জন্য।

### Archive Utility:

ডেটা আর্কাইভ করলে কোনো স্পেস কমবে না। তবে, বিভিন্ন মেটা ডাটার `inode, name, security, block` জন্য কিছু স্পেস কমতে পারে। আর্কাইভের এক্সটেনশন সাধারণত `.tar` ফরম্যাটে হয়ে থাকে অর্থাৎ ফাইলের নামের শেষে `.tar` থাকবে। উদাহরণঃ

### Archive.tar

Compression/Archive Utilities:

- Windows: zip, rar, 7zip
- linux: .gz, .bz2, .xz optional (.zip, .rar)

### Archive + Compression:

- Backup.tar.gz
- Backup.tar.bz2
- Backup.tar.xz

### Compression Ratio:

- Main File: 30MB
- Archive(tar): 30MB
- Compression: .gz(7MB), .bz2(6MB), .xz(4MB)

`du -sh /etc` `etc` ডিরেক্টরির সাইজ দেখা যাচ্ছে 30MB

# Archive (tar):

আমরা `tar` কমান্ড ব্যবহার করে আর্কাইভ এবং কম্প্রেশন উভয় কাজই করতে পারি এবং এটার প্যাকেজ (RPM) `tar` সিস্টেমে ডিফল্ট ভাবে ইন্সটল থাকে।

### tar কমান্ডের সাথে যে সকল অপশন ব্যবহার করা যাবেঃ

-c = Create
-x = extract
-t = list
-v = verbose
-f = files
-z = gzip
-j = bzip2
-J = xz
-C = change

- bunzip2 > bzip2 > bz2 -> তিনটা একই ফরম্যাট
- xzip > xz -> এই দুইটা একই ফরম্যাট
- gunzip > gzip > gz এই তিনটা একই ফরম্যাট

```
tar -cvf etcarchive.tar etc
ls
du -sh /etc
```

নোটঃ আর্কাইভ করার পরে আমরা `ls` কমান্ড দিয়ে দেখলাম `etcarchive.tar` নামে লাল রঙয়ের একটা (tar) ফাইল তৈরি হয়েছে। যেটার সাইজ `27 MB` অর্থাৎ মূল ফাইল `etc` ডিরেক্টরি থেকে মেটাডাটার `inode, name, security, block` জন্য কিছু স্পেস কম হয়েছে।

### Multiple File Archive to single File:

`touch file1 file2 file3`

- `tar -cvf files.tar file1 file2 file3` একাধিক ফাইলকে আর্কাইভ করে একটি ফাইলে রুপান্তর করার জন্য।
- `tar -tvf files.tar` আর্কাইভের মধ্যে কি আছে সেট দেখার জন্য `-t` অপশন।

### Arcive extract:

আর্কাইভ থেকে ফাইল এক্সট্রাক্ট (extract) করার জন্য, প্রথমে মূল ডিরেক্টরি `/etc` রিমুভ করে নেওয়া হবে।

```
rm -rf etc
ls
tar -xvf etcarchive.tar
ll
```

### Archive + Compress:

আমরা তিন (০৩) পদ্ধতিতে আর্কাইভ সাথে কম্প্রেসড করতে পারি।

- `Gzip (backup.tar.gz)` - এক্ষেত্রে `tar` কমান্ডের সাথে `-z` অপশন যোগ করতে হবে।
- `Bzip2 (backup.tar.bz2)` - এক্ষেত্রে `tar` কমান্ডের সাথে `-j` অপশন যোগ করতে হবে।
- `XZ (backup.tar.xz)` - এক্ষেত্রে `tar`' কমান্ডের সাথে `-J` অপশন যোগ করতে হবে।

### Archive + Gzip compress:

```
tar -czvf backup.tar.gz etc
ll
```

### Archive + Bzip2 compress:

```
tar -cjvf backup.tar.bz2 etc
ll
```

### Archive + XZ compress:

```
tar -cJvf backup.tar.xz etc
ll
```

`du -sh *` সকল ফাইলের সাইজ দেখার জন্য।

### Extract:

```
rm -rf etc
tar -xzvf etcbackup.tar.gz
ll
```

### extract: (.bz2)

```
rm -rf etc
tar -xjvf etcbackup.tar.bz2
ls
```

### extract: (.xz)

```
rm -rf etc
tar -xJvf etcbackup.tar.xz
ls
```

`tar -tf archive_file_name | less` আর্কাইভ ফাইলের ভিতরের ফাইলের লিস্ট দেখার জন্য।

`man tar | egrep "bzip|xz.gzip"` আর্কাইভ করার কমান্ড ভুলে গেলে অথবা অপশনস ভুলে গেলে।

`file file_name` কোন ফাইল আসলে সেটি ফাইল অথবা আর্কাইভ কিনা সেটা চেক করার জন্য।

`zip -r /opt/etc.zip /etc` জিপ করার কমান্ড

`unzip etc.zip` আনজিপ করার কমান্ড

### File Backup Local to Remote System:

- FTP Server
- NFS Server
- Samba (CIFS) Server
- SCP (Secure Copy) - SFTP
- Rsync

### Secure Copy (scp) from server: (From Node1)

আমরা চাইলে লিনাক্স সিস্টেম থেকে আরেক লিনাক্স সিস্টেমে ডেটা ট্রান্সফার করতে পারি। এক্ষেত্রে 'scp' কমান্ড ব্যবহার করতে হবে। ডেটা ট্রান্সফারের ক্ষেত্রে সিকিউর ফাইল ট্রান্সফার প্রটোকল SFTP (port 22) ব্যবহার করা হয়।

লোকাল সিস্টেম `node1` থেকে ফাইল রিমোট সিস্টেমে `node2` পাঠানোর জন্য নিচের কমান্ডঃ

### Secure Copy (scp) to node2:

`scp backup.tar.gz student@192.168.0.106:/tmp` (node2 সার্ভারে পাঠানো হয়ছে)

`scp student@192.168.0.106:/etc/passwd /backup` রিমোট সিস্টেম `node2` থেকে ফাইল কপি করে লোকাল সিস্টেমে `node1` নিয়ে আসা হয়েছে।

### File Copy using Rsync:

Here's a shortlist of features rsync offers.

- Directory copying
- Easy backup configuration
- Can work over SSH
- Can run as daemon/server
- File permission retention

Rsync stands for the term RemoteSync. Despite the name, it can handle file synchronization remotely and locally. The term rsync is also used to refer to the rsync protocol that rsync uses for syncing.

## Rsync Command structure:

### rsync <options> <SRC> <DST>

Rsync করার জন্য `node2` সিস্টেম `CentOS 9` ব্যবহার করা হবে।

`yum install rsync -y`

### Rsync within Local System Directories:

```
mkdir /backup -p
cd /backup
mkdir centos redhat
touch centos/file{1..10}
ls centos
rsync -av centos redhat; centos এর সাথে 'redhat' এর sync করা হল।
ls redhat/centos
touch centos/file4
rsync -av centos redhat
ls redhat/centos
```

### Rsync within Remote System Directories:

```bash
rsync -arv centos student@192.168.0.106:/tmp
```

### Optional (যদি আলাগা পোর্ট নাম্বার দিয়ে পাঠানো হয়)

```bash
rsync -av --rsh='ssh -p 2222' centos student@192.168.0.106:/tmp
```

```
a = archive
v = verbose
r = recursively
```

### Move to Remote Server:

```
cd /tmp
ls
```

## Working with Linux Log Files:

বিভিন্ন ধরনের লগ ফাইল (Events & Messages) সিস্টেম, অ্যাপ্লিকেশন, ইউজার কর্তৃক তৈরি হয়ঃ

- Kernel & Boot Log
- Application Log (http, mail, samba, crond)
- User login info (ssh, user login)
- Audit Log

লগ ফাইল সমূহ `cat, tail, less, grep, head` কমান্ড দিয়ে দেখা যায়। কিছু কিছু লগ ফাইল বাইনারী অবস্থায় থাকে, এক্ষেত্রে ফাইলে এনক্রিপ্টেড অবস্থায় থাকে। ফলে, ভিউ করা যায় না। সেক্ষেত্রে কমান্ড ব্যবহার করে ভিউ করতে হয়।

- `/var/log/messages` সিস্টেমের বুট এবং কার্নেল মেসেজ সম্পর্কিত লগ ফাইল জমা হয় এখানে।
- `/var/log/secure` SSH সার্ভিসের সকল লগ জমা হয় এই ফাইলে।
- `/var/log/maillog` Mail সার্ভার সম্পর্কিত সকল লগ `incoming/outgoing` এখানে পাওয়া যায়।
- `/var/log/dmesg` ডিভাইস ড্রাইভার সম্পর্কিত সকল লগ এখানে জমা হয়।
- `/var/log/cron` Cronjobs সম্পর্কিত সকল লগ এখানে জমা হয়।
- `/var/log/dnf.log` লিনাক্স প্যাকেজ ম্যানেজমেন্ট সিস্টেম (YUM) এর লগ এখানে জমা হয়।
- `/var/log/boot.log` সিস্টেম বুটিং সম্পর্কিত সকল লগ জমা হয় এখানে।

রিয়াল টাইম লগ দেখার জন্য `tail -f /var/log/messages`

গ্রাফিক্যালি সিস্টেমের স্ট্যাটাস / লগ দেখার জন্য আমরা ফ্রি টুল ইউজ করতে পারি।

```
nfsen
cacti
ManageEngine
LibreNMS
ELK (Kibana)
Solarwinds
Zabbix
Obervium
Nagios
OpenNMS
Grafana
Ichinga
```

সিস্টমের সকল ডিভাইস ড্রাইভার সম্পর্কিত সকল লগ ফাইল ফাইল নিচের কমান্ড দিয়ে দেখা যাবেঃ

```
dmesg
dmesg | grep scsi
```

### To show all event messages, use:

Systemd-journald saves the events and messages in a binary format that cannot be read with a text editor.

```
journalctl
journalctl -u sshd
journalctl -u firewalld
journalctl -u nfs-server
```

### System Kernel and Boot Log:

`tail -f /var/log/messages`

### SSH Login Users Info:

`cat /var/log/secure`

## Tuned

To maximize the end-to-end performance of services, applications and databases on a server, system administrators usually carry out custom performance tunning, using various tools, both generic operating system tools as well as third-party tools. One of the most useful performance tuning tools on CentOS/RHEL/Fedora Linux is Tuned.

`yum install tuned -y` (CentOS Node)

After the installation, you will find following important tuned configuration files.

- `/etc/tuned` – tuned configuration directory.
- `/etc/tuned/tuned-main.conf` – tuned main configuration file.
- `/usr/lib/tuned/` – stores a sub-directory for all tuning profiles.

Now you can start or manage the tuned service using following commands.

```
systemctl start tuned
systemctl enable tuned
systemctl status tuned
```

এখন `tunde-adm` কমান্ড ব্যবহার করে `tuned` প্রোফাইল ম্যানেজ করা যাবে।

### পুর্বে উল্লেখিত (Pre-defined) প্রোফাইল সমূহের লিস্ট দেখার জন্য নিচের কমান্ডঃ

`tuned-adm list`

### বর্তমানে একটিভ প্রোফাইল দেখার জন্য নিচের কমান্ডঃ

`tuned-adm active`
Current active profile: virtual-guest

### প্রোফাইল পরিবর্তন করার জন্য নিচের কমান্ডঃ

```
tuned-adm profile virtual-host
tuned-adm active
```

### ডিফল্ট প্রোফাইল লোড করার জন্য নিচের কমান্ডঃ

`tuned-adm recommend`

`tuned-adm auto_profile` রিকমেন্ডিড প্রফাইল সেইভ করার জন্য।

`tuned-adm profile_info`

`tuned-adm off` প্রোফাইল বন্ধ রাখার জন্য নিচের কমান্ডঃ

`tuned-adm active` প্রোফাইল চালু করার জন্য নিচের কমান্ডঃ

`find / -type d -iname "tuned"`

`/usr/lib/tuned` কাস্টম প্রফাইল তৈরি করার জন্য এই লোকেশনে একটা ফাইল তৈরি করে সেখানে স্ক্রিপ্ট লিখতে হবে।
