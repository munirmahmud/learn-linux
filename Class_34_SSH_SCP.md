# RHCSA Class 34: SSH, SCP

### What is OpenSSH?

`SSH` এর পূর্ণ অর্থ হচ্ছে `Secure Shell`। `SSH` ক্লায়েন্ট-সার্ভার আর্কিটেকচার পদ্ধতিতে কাজ করে। `SSH` অ্যাপ্লিকেশন বা প্রটোকল ব্যবহার করে আমরা রিমোট ডিভাইস রাউটার, ফায়ারওয়াল, বা সার্ভারে লগইন করতে পারি। এটা `Telnet` অ্যাপ্লিকেশন/প্রটোকলের মত রিমোট অ্যাডমিনিস্ট্রেশনের জন্য ব্যবহার হয়, কিন্ত `Telnet` সেশন সিকিউর হয় না, অর্থাৎ `Telnet` প্রটোকল `Plain Text` পদ্ধতিতে সেশন তৈরি করে, যার ফলে রিমোট ডিভাইসের `Server/Router/Firewall` লগইন ইউজার নাম এবং পাসওয়ার্ড সহজে হ্যাক হয়ে পারে। অপরদিকে `SSH` প্রটোকল সার্ভার এবং ক্লায়েন্টের মধ্যে সিকিউর `Encrypted` সেশন তৈরি করে, ফলে রিমোট সার্ভার / ডিভাইস এবং ক্লায়েন্টের মাঝে ইউজার নাম, পাসওয়ার্ড এবং ডাটা ট্রান্সফার এনক্রিপ্টেড `Cipher Text` পদ্ধতিতে হয়ে থাকে। `SSH` প্রটোকলের ডিফল্ট পোর্ট নাম্বার `22` এবং এটা একটি `TCP Protocol` ভিত্তিক সার্ভিস, তবে এটা পোর্ট নাম্বার পরিবর্তন করে কাস্টম পোর্ট ব্যবহার করা যায়।

`RHEL/CentOS/Rocky 9` অপারেটিং সিস্টেমে ডিফল্ট ভাবে `SSH` এনাবেল থাকে, কিন্ত `root` ইউজার এক্সেস থাকে না। সিকিউরিটি ইস্যুর কারনে ডিফল্ট কনফিগারেশন `Default Port, Root Login, Password-based authentication` পরিবর্তন করে নিতে হবে। যার ফলে নির্দিষ্ট ইউজার বা হোস্ট ছাড়া অন্য কেউ লগইন করতে পারবে না।

### Module Objectives:

- `RHEL/CentOS Stream 9` -এ `SSH` প্রটোকল/অ্যাপ্লিকেশনের ব্যবহার।
- `SSH` এর মাধ্যমে লিনাক্স এবং উইন্ডোজ ক্লায়েন্ট থেকে লিনাক্স `RHEL/CentOS 9` সার্ভারে লগইন করা।
- `SSH` এর `password-based` এবং `key-based` অথেনটিকেশন সম্পর্কে বিস্তারিত আলোচনা করা।
- `SSH` এর `Private` এবং `Public key` সম্পর্কে বিস্তারিত আলোচনা।
- `SSH key pair` তৈরি করা এবং রিমোট সার্ভারে পাঠানো।
- `SSH` দিয়ে লিনাক্স সার্ভারে `root` ইউজারের সরাসরি এক্সেস বন্ধ করা।
- `Password-based` অথেনটিকেশন বন্ধ করা এবং শুধুমাত্র `key-based` অথেনটিকেশন চালু রাখা।
- `SSH` এর ডিফল্ট পোর্ট (22) পরিবর্তন করে কাস্টম পোর্ট ব্যবহার করে সিকিউরিটি প্রদান করা।

### Reference Tables:

- `Port no: 22 (default)`, কিন্ত পরিবর্তন করা যাবে।
- `Protocol: TCP`
- `Package (RPMs): openssh-server (node2), openssh-client(node1)`
- `Configuration file: /etc/ssh/sshd_config (node2)`
- `Daemon/Service: sshd`

## Step by step configuration:

### Step 01: Host Name Change:

`hostnamectl set-hostname node1.example.com`

`bash`

### Step 02: Check IP Address Configure:

`ifconfig`

### Step 03: RPM Query, install (node2)

`rpm -qa | grep openssh-server`
openssh-server-8.7p1-28.el9.x86_64

### Step 04: Service Restart

- `systemctl start sshd.service`
- `systemctl enable sshd.service`
- `systemctl status sshd.service`

`netstat -ntlp | grep ssh`

### Step 05: Create SSH Login User and Set Password:

- `useradd admin1`
- `useradd user1`
- `passwd admin1`
- `passwd user1`

### SSH Client:

- Linux Client (defautl installed)
- Windows Client (putty, ssh client) > putty > ssh > ip > port (22)
- Move to linux client machine
- ping 192.168.0.106 (ssh server)
- Command syntax: SSH <remote host IP/Name> `ssh user_name@ip/hostname`

### SSH Login with Root User (SSH Linux Client):

`ssh admin1@192.168.0.106`

Are you sure you want to continue connecting (yes/no)? yes
admin1@192.168.0.106's password: **\*\*** (remote PC)

```
who
su -
```

Password: \*\*\*\*

### SSH Authentication:

ডিফল্ট ভাবে `RHEL 9` -এ `root` ইউজারের লগিন এক্সেস থাকে না। তবে চাইলে নির্দিস্ট কিছু ইউজারকে `ssh` ব্যবহার করার প্রিভিলেজ দিতে পারি।

`vim /etc/ssh/sshd_config`

`:set nu`

```
37 # Authentication:
38 PermitRootLogin no (yes) <------------- যদি 'root' ইউজারের এক্সেস দরকার হয়।
39 AllowUsers admin1
```

`AllowUsers` ইউজ করলে এখানে যেই ইউজার মেনশন করা থাকবে সেগুলো ছাড়া বাকি ইউজাররা `ssh key` দিয়ে লগইন করতে পারবে না।

`systemctl reload sshd`

এখন `host/node2` মেশিন থেকে টেস্ট করা হবে।

- `ssh root@192.168.0.106` - (Permission denied) - যদি `PermitRootLogin no` থাকে
- `ssh user1@192.168.0.106` - (Permission denied)
- `ssh admin1@192.168.0.106` - allowed

### Type of SSH Authentication:

- Password authentication
- Public key authentication (main authentication method)
- 2FA Authentication

Password Authentication পদ্ধতিতে কিছু সিকিউরিটি ইস্যু আছে। যেমন বিভিন্ন ভাবে পাসওয়ার্ড ক্র্যাক হতে পারে। আরো কিছু সমস্যা নিচে উল্লেখ করা হয়েছেঃ

- Dictionary Attack
- Brutforce attack
- MiM Attack
- Password lost
- Password Forget
- Password Manage (10+ server)

### Key Based Authentication:

- Public key: প্রত্যেক রিমোট সিস্টেমের কাছে থাকবে এটা।
- Private key: প্রত্যেক লোকাল সিস্টেম `ssh client` এর কাছে থাকবে এটা।

### Key-pair তৈরি করার জন্য যে সকল এলগরিদম সমূহ ব্যবহার করা হয়ঃ

- 1. RSA
- 2. DSA (deprecated)
- 3. ECDSA
- 4. ed25519

- 1. RSA means Rivest, Shamir, Adleman. These are the inventors of the popular RSA Algorithm. The RSA algorithm is based on public-key encryption technology which is a public-key cryptosystem for reliable data transmission.
- 2. Digital Signature Algorithm
- 3. Elliptic Curve Digital Signature Algorithm. The Elliptic Curve Digital Signature Algorithm (ECDSA) is a Digital Signature Algorithm (DSA) which uses keys derived from elliptic curve cryptography (ECC). It is a particularly efficient equation based on public key cryptography (PKC).

উপরের এ্যালগরিদম এর মধ্যে `RSA (3072 bit)` অধিক সিকিউর এবং বেশি ব্যবহার করা হয়।

### Password Less ssh login:

```
cd
ls
ls -a
cd .ssh - SSH সম্পর্কিত
ls
known_host (list of known hosts)
```

`known_host` ফাইলে মূলত সার্ভার এ্যাক্সেসের ফুটপ্রিন্ট থাকে।

`ssh-keygen` (Enter 3 Times)

`id_rsa.pub id_rsa` নতুন দুইটা ফাইল তৈরি হবে।

```
cat id_rsa
cat id_rsa.pub
```

```
ssh-copy-id admin1@172.25.11.X (nod2)
ssh admin1@172.25.11.X
```

```
cd .ssh
ls
authorized_keys
```

```
cat authorized_keys - same as public key of hostX
exit
```

### SCP: Secure Copy

এক সিস্টেম থেকে অন্য সিস্টেমে কোন ফাইল ট্রান্সফার করার জন্য ইউজ করা হয়

`scp <source> <destination>`

- `scp /etc/passwd munir@192.168.0.106:/opt/`
- `scp -r /etc/ munir@192.168.0.106:/opt/` কোন ডিরেক্টরি পাঠানোর জন্য `-r` ইউজ করা হয়।
- `scp /etc/passwd munir@192.168.0.106:/opt/password.txt` ফাইল পাঠানোর পরে রিনেইম হয়ে যাবে।
- `scp munir@192.168.0.106:/etc/passwd /opt` অন্য সিস্টেম থেকে ফাইল কপি করে আনার কমান্ড।
- `ssh munir@192.168.0.106 "ls /"` অন্য সিস্টেমের রুট পার্টিশনে কি কি আছে তা দেখা যাবে।
- `ssh munir@192.168.0.106 "yum install httpd -y"` অন্য সিস্টেমে কোন প্যাকেজ ইন্সটল করার জন্য।
- `ssh -P 2025 /etc/ munir@192.168.0.106:/opt/` কাস্টম পোর্ট ইউজ করে কোন ফাইল বা ডিরেক্টরি কপি করা।

### Change Default Port:

SSH এর ডিফল্ট পোর্ট TCP `22` তবে, সিকিউরিটি ইস্যুর কারণে, ডিফল্ট পোর্ট পরিবর্তন করে কাস্টম পোর্ট ব্যবহার করা উচিত।

```
netstat -ntlp | grep ssh
ss -ltn
```

```
vim /etc/ssh/sshd_config
:set nu
```

`21 #Port 22` old
`21 Port 2025` remove '#'

```
systemctl restart sshd.service
setenforce 0 -> SELinux Permissive mode
systemctl restart sshd.service
```

### Verify current SSH port:

```
netstat -ntlp | grep ssh কত পোর্টে ssh রান হচ্ছে তা দেখার জন্য।
ss -ltn
```

### Check Firewall Status:

```
systemctl status firewalld
systemctl stop firewalld
```

### ==> Move to node1

`ssh munir@192.168.0.106` (default port) - Connection Refused
`ssh -p 2025 munir@192.168.0.106` যদি ইউজার `munir` এবং নতুন পোর্ট `2025` দিয়ে লগিন করতে চাই।

### Homework:

- Allow user 'admin2' to login through ssh
- Generate key pair on windows os
