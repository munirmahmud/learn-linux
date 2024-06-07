# RHCSA Class 32: SELinux - Security Enhanced Linux

- What is SELinux
- SELinux Modes
- Manage the SELinux Policy
- Activate & deactivate SELinux Policy Rules
- Use SELinux Log Analysis
- Change the SELinux Enforcement Mode
- The SELinux context

## Security Model

`DAC: Discretionary Access Control` অধিকাংশ লিনাক্স সিস্টেম অ্যাডমিন লিনাক্সের স্ট্যান্ডার্ড ফাইল পার্মিশনের `User, Group, Others` এর সাথে সুপরিচিত, যেটা সিকিউরিটির ভাষায় `DAC` এর অন্তভুক্ত। `DAC` ভিত্তিক সিকিউরিটি ব্যবহার করা সহজ `Flexible`, এটা সহজেই এপ্লাই করা যায়, এবং এই ধরনের পার্মিশন ইউজার কর্তৃক ম্যানেজ করা হয়। অর্থাৎ, কে ফাইল `read` করতে পারবে, কে `write` করতে পারবে, এবং কে `Delete` করতে পারবে (এছাডাও আছে `SUID, GUID, Sticky Bit`) সেই ভাবে ফাইল ওনার সিকিউরিটি সেট করতে পারে। তবে, `DAC` ভিত্তিক সিকিউরিটি সিস্টেম কম সিকিউর।

`MAC: Mandatory Access Control` ভিত্তিক সিকিউরিটি মডেলে ইউজারের কোনো ক্ষমতা থাকে না। এক্ষেত্রে সিস্টেম কর্তৃক সকল রিসোর্স এক্সেস কন্ট্রোল, পার্মিশন ইত্যাদি ম্যানেজ করা হয়। এক্সেস পার্মিশন সমূহ সিস্টেমের পুর্ব নির্ধারিত বিভিন্ন পলিসির মাধ্যমে ম্যানেজ করা হয়। যেহেতু পলিসি সমূহ সিস্টেম কর্তৃক `Centrally` ম্যানেজ করা হয়, এক্ষেত্রে ইউজারের কোনো কিছু পরিবর্তন করার সুযোগ থাকে না। এইজন্য `MAC` ভিত্তিক সিকিউরিটি মডেল `DAC` এর চেয়ে অধিক সিকিউর। `AppArmor, SELinux` ইত্যাদি `MAC` ভিত্তিক সিকিউরিটি সিস্টেম। ফেডোরা ভিত্তিক ডিস্ট্রিবিউশন এর জন্য `SeLinux` আর ডেবিয়ান ভিত্তিক ডিস্ট্রিবিউশন এর জন্য `AppArmor` সিকিউরিটি পলিসি অ্যাপ্লাই করা হয়।

## SELinux কি?

`SELinux` এর পুর্ন অর্থ `Security-Enhanced Linux` যেটা `MAC` সিস্টেম সমর্থন করে। `SELinux` সিকিউরিটি লিনাক্স কার্নেলের সিকিউরিটি সাব-সিস্টমের অন্তর্ভুক্ত। `SELinux` সিকিউরিটি `NSA: National Security Agency` কর্তৃক ডেভলপ করা হয় এবং `RHEL 4` থেকে অর্থাৎ লিনাক্স কার্নেল `2.6` ভার্শনের সাথে প্রথম `SELinux` এর ব্যবহার শুরু হয়।

`SELinux` সিকিউরিটি লিনাক্স সিস্টেমের মিলিটারি গ্রেড সিকিউরিটির অন্তর্ভুক্ত। `SELinux` পলিসি বা বুলিয়ান `Boolean` এর মাধ্যমে সিস্টেমের বিভিন্ন ধরনের রিসোর্স, যেমনঃ `File, Application, Service, Process, Port` সমূহ রুট লেভেল থেকে নিয়ন্ত্রণ করা যায়।

## SELinux Status:

- Enable (Enforcing & Permissive)
- Disabled

## SELinux Enable Modes:

- Enforcing Mode (1)
- Permissive Mode (0)

- Enforcing: এটা `SELinux` এর ডিফল্ট মোড, `Enforcing` মোডের মাধ্যমে পলিসি সমূহ প্রয়োগ (Enforce) করা হয়। `SELinux` সিকিউরিটি পলিসি কর্তৃক অনুমোদিত ব্যাতিত কোনো ইউজার বা প্রোগাম কাজ করবে না।

- Permissive: এই মোডে `SELinux` পলিসি সমূহ সিস্টেমে লোডিং অবস্থায় এবং এক্টিভ থাকে, কিন্ত কোনো পলিসি প্রয়োগ (Enforce) হয় না। তবে, `SELinux` এর সম্পর্কিত `Security Violations` সকল লগ ফাইল সিস্টেমে জমা থাকে। ফলে, `System Testing, Audinting, Protection, & Troubleshooting` করতে সুবিধা হয়।

- Disabled: কোনো `SELinux` সিকিউরিটি পলিসি কার্নেল কর্তৃক লোড হয় না। `Disable` অর্থ `SELinux` সার্ভিস সম্পূর্ণ বন্ধ থাকে।

## SELinux Information:

`getenforce` এর মাধ্যমে `SELinux` এর বর্তমান অবস্থা জানা যায়।

নোটঃ `Disable` থাকলে এই কমান্ড কাজ করবে না।

## Set SELinux Temporary Enforcing & Permissive:

`setenforce` এই কমান্ডের মাধ্যমে SELinux এর মোড `Enforcing` বা `Permissive` করা যাবে।

`setenforce 0 | 1` (0) জিরো অথবা (1) দিয়ে `SELinux` মোড চেঞ্জ করা যায়।

`getenforce` বর্তমানে `SELinux` কোন মোডে আছে তা দেখার জন্য।

নোটঃ এখানে (0) জিরো অর্থ `Permissive` এবং (1) এর অর্থ `Enforcing` আর `getenforce` কমান্ডের মাধ্যমে `SELinux` এর বর্তমান অবস্থা জানা যাবে।

## Current SELinux Status:

`sestatus` SELinux সম্পর্কিত সকল তথ্য পাওয়া যাবে।

- `SELinux status: enabled - SELinux` এর বর্তমান কি অবস্থায় `Disabled or Enabled` আছে।
- `SELinuxfs mount: /sys/fs/selinux - SELinux` এর ফাইল সিস্টেম যে লোকেশনে মাউন্ট করা আছে।
- `SELinux root directory: /etc/selinux` - যে লোকেশনে `SELinux` সম্পর্কিত কনফিগারেশন ফাইল আছে।
- `Loaded policy name: targeted` - দুই ধরনের `Policy` আছে `Targetd & Strict`. নির্দিস্ট ডেমন বা আপ্পলিকেশন `targeted` এবং সব ধরনের ডেমনের ক্ষেত্রে `strict`

- `Current mode: enforcing - SELinux` এর বর্তমান মোড।
- `Mode from config file: enforcing` - SELinux কনফিগারেশন ফাইলে বর্তমানে যে মোডে আছে।
- `Policy MLS status: enabled`
- `Policy deny_unknown status: allowed`
- `Max kernel policy version: 33 - SELinux` পলিসির যে ভার্শনটি বর্তমানে ব্যবহৃত হচ্ছে।

## Change SELinux Mode:

স্থায়ী ভাবে SELinux মোড পরিবর্তন করার জন্য নিচের ফাইলটি এডিট করে প্রয়োজন মত SELinux মোড `Enforcing Permissive & Disabled` সেট করে, ফাইল সেভ করে রিবুট দিতে হবে। সিস্টেম রিবুটের সময় সিস্টেমের সকল ফাইল/ডিরেক্টরি/সার্ভিস/পোর্ট/প্রসেস SELinux এর নতুন মোড কর্তৃক পুনরায় লেভেল (relabel) হবে। এইজন্য মোড পরিবর্তন করার পরে অবশ্যই রিবুট দিতে হবে।

- `vim /etc/selinux/config` SELinux configuration ফাইল লোকেশন।
- `SELINUX=enforcing`
- `reboot`

## The SELinux Context:

`SELinux` এর ব্যবহার লিনাক্স/ইউনিক্স সিস্টমে ব্যবহৃত প্রচলিত সিকিউরিটি সিস্টেম থেকে সম্পূর্ণ আলাদা। `SELinux` সিকিউরিটি বিভিন্ন ধরনের সিকিউরিটি `Context` কর্তৃক সিস্টমের সকল রিসোর্স `Processes, Files, Service, Ports` লেভেল করা থাকে।

নিচের কমান্ডের মাধ্যমে ফাইল/ডিরেক্টরির `SELinux` সিকিউরিটি কন্টেক্সট সম্পর্কে জানা যাবেঃ

- `ls -Z`
- `system_u:object_r:admin_home_t:s0 anaconda-ks.cfg`
- `ls -Z /`
- `ls -dlZ /tmp/`
- `drwxrwxrwt. root root system_u:object_r:tmp_t:s0 /tmp/`
- `ls -lZ /home`
- `drwx------. david 1000 unconfined_u:object_r:user_home_dir_t:s0 david`
- `drwx------. 5001 5001 unconfined_u:object_r:user_home_dir_t:s0 john`

নোটঃ উপরের কমান্ড দেওয়ার পরে, প্রতিটি ফাইল/ডিরেক্টরির গায়ে নতুন একটি লেভেল দেখা যাচ্ছে, এক একটি পার্টকে লেভেল বলা হয়। সকল লেভেলকে এক সঙ্গে SELinux Context বলা হয়।

## SELinux Label:

=> system_u:object_r:syslog_t:s0 (MLS)

- SELinux User - (\_u)
- SELinux Role - (\_r)
- SELinux Type - (\_t)
- SELinux Sensitivity - (s0-s15)

## RHEL 9 provides the following packages for working with SELinux:

policies: selinux-policy-targeted, selinux-policy-mls
tools: policycoreutils, policycoreutils-gui, libselinux-utils, policycoreutils-python-utils, setools-console, checkpolicy

## Related Package Install:

```bash
yum install setools-console -y
yum install policycoreutils\* -y
```

উপরের প্যাকেজ ইন্সটলের পরে `seinfo` কমান্ড ব্যবহার করা যাবে। `seinfo` কমান্ডের মাধ্যমে `SELinux` সম্পর্কিত বিভিন্ন কুয়েরী যেমনঃ `Users, Roles, Types, Attributes, Booleans` ইত্যাদি বিষয়ের তথ্য পাওয়া যাবে। নিচে `seinfo` কমান্ড ব্যবহার করে `SELinux` সম্পর্কিত বিভিন্ন তথ্য বের করার পদ্ধতি দেখানো হয়েছেঃ

## SELinx User:

`SELinux Context` এর প্রথম অংশ হচ্ছে `SELinux` ইউজার পার্ট। অর্থাৎ, প্রথম অংশ দেখে ইউজার `SELinux` ইউজার আইডেন্টিফাই করা যাবে। RHEL সিস্টমে আট (০৮) ধরনের SELinux ইউজার থাকে। একজন SELinux ইউজার একাধিক SELinux রোল (Role) ম্যানেজ করতে পারে। নিচের কমান্ড ব্যবহার করে সিস্টেমে কি কি SELinux ইউজার আছে, এই সম্পর্কে জানা যাবে।

`seinfo -u`

- Users: 8
- sysadm_u
- system_u
- xguest_u
- root
- guest_u
- staff_u
- user_u
- unconfined_u

## Role:

`SELinux Context` এর দ্বিতীয় অংশ হচ্ছে `Role` এবং `Role` সমূহ `\_r` দিয়ে উল্লেখ করা হয়। একজন `SELinux` ইউজার একাধিক `Role` ম্যানেজ করতে পারে। `SELinux` ইউজার কর্তৃক যে সকল `Role` ম্যানেজ করা হয়, সেটা জানার জন্য নিচের কমান্ড।

`seinfo -r`

- Roles: 15
- auditadm_r
- dbadm_r
- guest_r
- logadm_r
- nx_server_r
- object_r
- secadm_r
- staff_r
- sysadm_r
- system_r
- unconfined_r
- user_r
- webadm_r
- xguest_r

## SELinux Contexts: (-t)

সিস্টেমে কি কি `security context (\_t)` এবং মোট কতগুলা সিকিউরিটি `Context` আছে সেটা জানার জন্যঃ

`seinfo -t` মোট কতগুলো `security context` আছে তার লিস্ট দেখা যাবে।

`seinfo -t | wc -l` মোট কতগুলো `security context` আছে তা দেখা যাবে।

`seinfo -t | grep sshd`

`yum install httpd -y`

```
systemctl restart httpd
systemctl enable httpd
systemctl status httpd
```

```bash
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload
```

`iptables -F`

After changing ssh or other port - from eng tuts
firewall-cmd --zone=public --add-port=2224/tcp

## Create a webpage:

```bash
echo "HEllo World" >> /var/www/html/index.html
```

Visit web page: http://172.25.11.X

## SELinux Testing: Example: 01

```
cd
ls -Zd ~
dr-xr-x---. root root system_u:object_r:admin_home_t:s0 /root
```

```
touch redhat
ls -Z redhat
-rw-r--r--. root root unconfined_u:object_r:admin_home_t:s0 redhat
```

```
cp redhat /var/www/html/centos
mv redhat /var/www/html/
ls -Zd /var/www/html/
drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html/
```

```
ls -Z /var/www/html/\*
-rw-r--r--. root root unconfined_u:object_r:admin_home_t:s0 redhat
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 centos
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 index.html
```

## Change SELinux Security Context: (Method-1)

`restorecon -v /var/www/html/redhat` প্যারেন্ট ডিরেক্টরি অনুযায়ী ফাইলটি `relabel` হবে।

`ls -Z /var/www/html/redhat`

`-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 redhat`

## Method 02: (Context Change)

`chcon -t httpd_sys_content_t /var/www/html/about.html` `chcon` মূলত সিঙ্গেল সিঙ্গেল ফাইলের জন্য ইউজ করা হয়।

`ls -lZ /var/www/html/about.html`

`semanage fcontext -l` context লিস্টের বিস্তারিত দেখার জন্য।

`semanage fcontext -l | grep "/var/www"` নির্দিষ্ট কোন ফাইল বা ডিরেক্টরির কন্টেক্সট দেখার জন্য।

```
semanage fcontext -a -t httpd_sys_content_t '/server(/.\*)?'
restorecon -Rv /server
```

প্যারেন্ট ডিরেক্টরি অনুযায়ী ভিতরের সকল ফাইল এবং ডিরেক্টরি এবং আরো যত ফাইল এবং ডিরেক্টরি তৈরি করা হবে সকল ফাইল এবং ডিরেক্টরির কন্টেক্সট একই হবে তাহলে এই কমান্ডটি রান করতে হবে।

`semanage fcontext -a -t httpd_sys_content_t '/server/index.html'` কোন সিঙ্গেল ফাইলের কন্টেক্সট চেঞ্জ করার জন্যও ইউজ করতে পারি।

অথবা নিচের কমান্ড রান করলেও একই কাজ করবে। এক্ষেত্রে সকল ফাইল এবং ডিরেক্টরি রি-লেভেল হবে।
`restorecon -Rv /server`

# SELinux Testing: Example: 02

`mkdir /server`

`ls -ldZ /server`

`drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 /server`

```
touch /server/index.html
echo "HEllo, from SELinux" >> /server/index.html
```

```
ls -lZ /server
-rw-r--r--. root root unconfined_u:object_r:default_t:s0 index.html
```

`vim /etc/httpd/conf/httpd.conf` Apache configuration ফাইল।

- `123 DocumentRoot "/server"`
- `134 <Directory "/server">`

`systemctl restart httpd`

=> Switch to DesktopX
=> Open Firefox Browser
=> http://192.168.0.106

এখন সাইট ভিজিট করলে ডিফল্ট হোম পেজ আসবে, কারণ `SELinux` এখন `Enforcing` মোডে আছে এবং `/server` ডিরেক্টরির সিকিউরিটি কন্টেক্সট (context) `http` সার্ভিসের সাথে মিল নাই। তবে আমরা যদি এটাকে `setenforce 0` কমান্ড দিয়ে `Permissive` মোড সেট করে দেই, তাহলে নতুন লোকেশন থেকে পেজ রান হবে।

=> http://192.168.0.106/index.html

এমতাবস্থায় `SELinux` Permissive মোডে রাখলে ওয়েব পেজ আসবে।

```
setenforce 0
getenforce
```

`http://192.168.0.106/index.html`

```
setenforce 1
getenforce
```

`http://192.168.0.106/index.html`

কিন্ত, আমাদেরকে সব সময় `SELinux` মোড `Enforcing` মোডে রেখে কাজ করতে হবে। নিচের কমান্ডের মাধ্যমে `httpd_sys_content_t` কন্টেক্সট (context) দিয়ে `/server` ডিরেক্টরি এবং এটার মধ্যে সকল সাব-ডিরেক্টরি, ফাইল সমূহের লেভেল পরিবর্তন করা হয়েছে।

```
semanage fcontext -a -t httpd_sys_content_t '/server(/.\*)?'
restorecon -Rv /server
getenforce
```

এখন যদি পুনরায় সাইট ভিজিট করা হয়, তাহলে ওয়েব সাইট দেখা যাবে।

`http://192.168.0.106/index.html`

## SELinux Port Labeling:

`SELinux` ফাইল এবং প্রসেস লেভেলে সিকিউরিটি `SELinux Policy` দেওয়ার পাশাপাশি নেটওয়ার্ক ট্রাফিকেও খুব শক্তভাবে মনিটরিং এবং সিকিউরিটি প্রদান করে। যেমন, বিভিন্ন ধরনের নেটওয়ার্ক সার্ভিস সমূহ `TCP/UDP` `SELinux` সিকিউরিটি কর্তৃক নিয়ন্ত্রণ করা হয়। উদাহরণ স্বরূপ `SSH` সার্ভিসের ডিফল্ট পোর্ট হিসেবে `22/TCP`, `HTTP` সার্ভিসের জন্য `80` এবং `HTTPs` সার্ভিসের জন্য ডিফল্ট পোর্ট `443` ইত্যাদি `SELinux` কর্তৃক লেভেল `bind` করা থাকে। যেমনঃ

- `ssh_port_t = 22/TCP`
- `http_port_t = 80/TCP & 443/TCP`

যখন সিস্টেমে বিভিন্ন সার্ভিসের ডিফল্ট পোর্ট পরিবর্তন করে যদি অন্য পোর্ট ব্যবহার করা হয়, তখন `SELinux` সিকিউরিটি কর্তৃক সেই সার্ভিস বন্ধ করে দেওয় হয়।

`netstat -ntlp | grep ssh` বর্তমানে `SSH` সার্ভিস কোন পোর্টে রান করছে সেটা দেখার জন্য।

`semanage port -l` SELinux কর্তৃক লেভেলকৃত পোর্টে সমূহের লিস্ট দেখা যাবে।

`semanage port -l | grep ssh` SELinux কর্তৃক লেভেলকৃত `SSH` সার্ভিসের পোর্ট নাম্বার দেখার জন্য।

`Output: ssh_port_t tcp 22`

`semanage port -l | grep http`

`semanage port -a -t ssh_port_t -p tcp 2025` SSH এর পোর্ট `22` এর পাশাপাশি 2025 পোর্টও লেভেল (bind) করা হল।

`semanage port -l | grep ssh`

`ssh_port_t tcp 2025, 22`

`Change SSH Default port '2025'`

সিকিউরিটি কন্টেক্সটে পোর্ট অ্যাড করার পরে ফায়ারওয়ালে ঐ পোর্টটি অ্যাড করে দিতে হবে।

## SELinux Boolean:

`SELinux Booleans` এক ধরনের কনফিগারেশন পদ্ধতি যেটা ব্যবহার করে অ্যাডমিনিস্ট্রেটর খুব সহজে `SELinux` পলিসি পরিবর্তন ছাড়াই ব্যবহার করতে পারবে।

`getsebool -a` সিস্টেমে যত `Boolean` আছে সকল লিস্ট দেখার জন্য।

- `semanage boolean -l`
- `semanage boolean -l | grep ftp` service must be enabled `ftp` এর বুলিয়ান দেখার জন্য
- `semanage boolean -l | grep httpd` service must be enabled `apache` এর বুলিয়ান দেখার জন্য

`off` মানে হল এটা ডিনাই করা আছে অর্থাৎ `SELinux` এর পলিসিতে পড়ে গেছে। আর `on` করে দিলে সেই নির্দিষ্ট সার্ভিসটি `by pass` মোডে এ্যালাউ করা আছে।

`semanage boolean -l -C`

যে কোনো Boolean এনাবেল করার জন্য `on` বা ডিজাবেল করার জন্য `off` করতে হবে।

## Boolean Enable করার জন্য নিচের কমান্ডঃ

`setsebool -P < boolean_name > on`

## Lab Demo:

এখানে `student` ইউজারের `home` ডিরেক্টরিতে একটি `webpage` হোস্ট করা হবে। উক্ত ওয়েব পেজটির হোস্টিং ডিরেক্টরি `SELinux Boolean` দিয়ে `enable` করতে হবে।

`vim /etc/httpd/conf.d/userdir.conf`

```
11 <IfModule mod_userdir.c>
17 # UserDir disabled
24 UserDir public_html
25 </IfModule>
```

`systemctl restart httpd`

Switch to student account

`su - student`

```
mkdir ~/public_html
echo 'This is student content on Node1' > ~/public_html/index.html
chmod 711 ~
```

## Student ইউজারের হোস্টিং এক্সেস করার জন্য নিচের URL অনুযায়ী চেক করতে হবেঃ

`http://192.168.0.106/~student/index.html` ভিজিট করতে হবে।

Output: An error message states that you do not have permission to access the file.

`semanage boolean -l | grep httpd_enable_homedirs`

httpd_enable_homedirs (off, off) Allow httpd to read home directories

Enable Boolean 'httpd_enable_homedirs' for user home directory:

`setsebool -P httpd_enable_homedirs on`

`getsebool httpd_enable_homedirs`
httpd_enable_homedirs --> on

Now Visit: `http://192.168.0.106/~student/index.html` ভিজিট করতে হবে।

## Homework:

Your company has decided to run a new web app. This application listens on ports 80/TCP and
8090/TCP. Port 2025/TCP for ssh access must also be available. All changes you make should
persist across a reboot.

`sealert -a /var/log/audit/audit.log`

<!-- ----------------------- -->

`ls -lZ file_name` কোন ফাইলের ফাইল কন্টেক্সট বা ট্যাগ দেখার কমান্ড।

`semanage fcontext -l | less` কি কি ধরনের কন্টেক্সেট আছে তা দেখা যাবে।

`semanage fcontext -l | grep "httpd"` কোন নির্দিষ্ট প্যাকেজের কন্টেক্সেট দেখার জন্য।

যদি কোন ফাইলে ম্যানুয়ালি `security context` অ্যাড করা হয় তাহলে `restorecon` কমান্ড রান করতে হবে।

- `touch abc.txt` একটা ফাইলে তৈরি করলাম।
- `ls -lZ abc.txt` ফাইলের কন্টেক্সেট চেক করলাম।
- `semanage fcontext -a -t httpd_sys_content_t /hello/abc.txt` ম্যানুয়ালি কন্টেক্সেট অ্যাড করলাম।
- `semanage fcontext -l | grep "abc.txt"` Semanage ডাটাবেইজ থেকে কন্টেক্সেট চেক করলাম।
- `restorecon -Rv /var/www/`
- `ls -lZ abc.txt`

প্রতিটি ডিরেক্টরিতে আলাদা আলাদা `security context` দেয়া থাকে।

যদি কোন ডিরেক্টরিতে security context অ্যাড করতে চাই।

`semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"`

`man semanage fcontext | grep web` সিস্টেম থেকে security context অ্যাড করার কমান্ড।

`/etc/httpd/conf/httpd.conf` Apache এর ডিফল্ট configuration file
