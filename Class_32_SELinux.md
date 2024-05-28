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

`MAC: Mandatory Access Control` ভিত্তিক সিকিউরিটি মডেলে ইউজারের কোনো ক্ষমতা থাকে না। এক্ষেত্রে সিস্টেম কর্তৃক সকল রিসোর্স এক্সেস কন্ট্রোল, পার্মিশন ইত্যাদি ম্যানেজ করা হয়। এক্সেস পার্মিশন সমূহ সিস্টেমের পুর্ব নির্ধারিত বিভিন্ন পলিসির মাধ্যমে ম্যানেজ করা হয়। যেহেতু পলিসি সমূহ সিস্টেম কর্তৃক `Centrally` ম্যানেজ করা হয়, এক্ষেত্রে ইউজারের কোনো কিছু পরিবর্তন করার সুযোগ থাকে না। এইজন্য `MAC` ভিত্তিক সিকিউরিটি মডেল `DAC` এর চেয়ে অধিক সিকিউর। `AppArmor, SELinux` ইত্যাদি `MAC` ভিত্তিক সিকিউরিটি সিস্টেম।

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

স্থায়ী ভাবে SELinux মোড পরিবর্তন করার জন্য নিচের ফাইলটি এডিট করে প্রয়োজন মত SELinux মোড 'Enforcing', 'Permissive' এবং 'Disabled' সেট করে, ফাইল সেভ করে রিবুট দিতে হবে। সিস্টেম রিবুটের সময় সিস্টেমের সকল ফাইল/ডিরেক্টরি/সার্ভিস/পোর্ট/প্রসেস SELinux এর নতুন মোড কর্তৃক পুনরায় লেভেল (relabel) হবে। এইজন্য মোড পরিবর্তন করার পরে অবশ্যই রিবুট দিতে হবে।

[root@node1 ~]# vim /etc/selinux/config
SELINUX=enforcing

[root@node1 ~]# reboot

## The SELinux Context:

SELinux এর ব্যবহার লিনাক্স/ইউনিক্স সিস্টমে ব্যবহৃত প্রচলিত সিকিউরিটি সিস্টেম থেকে সম্পূর্ণ আলাদা। SELinux সিকিউরিটি বিভিন্ন ধরনের সিকিউরিটি ‘Context’ কর্তৃক সিস্টমের সকল রিসোর্স (Processes, Files, Service, Ports) লেভেল করা থাকে।

নিচের কমান্ডের মাধ্যমে ফাইল/ডিরেক্টরির SELinux সিকিউরিটি কন্টেক্সট সম্পর্কে জানা যাবেঃ

[root@node1 ~]# ls -Z
system_u:object_r:admin_home_t:s0 anaconda-ks.cfg

[root@mynms ~]# ls -Z /

[root@node1 ~]# ls -dlZ /tmp/
drwxrwxrwt. root root system_u:object_r:tmp_t:s0 /tmp/

[root@node1 ~]# ls -lZ /home
drwx------. student 1000 unconfined_u:object_r:user_home_dir_t:s0 student
drwx------. 5001 5001 unconfined_u:object_r:user_home_dir_t:s0 tarek

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

[root@node1 ~]# yum install setools-console -y
[root@node1 ~]# yum install policycoreutils\* -y

উপরের প্যাকেজ ইন্সটলের পরে 'seinfo' কমান্ড ব্যবহার করা যাবে। 'seinfo' কমান্ডের মাধ্যমে SELinux সম্পর্কিত বিভিন্ন কুয়েরী যেমনঃ Users, Roles, Types, Attributes, Booleans, ইত্যাদি বিষয়ের তথ্য পাওয়া যাবে। নিচে 'seinfo' কমান্ড ব্যবহার করে SELinux সম্পর্কিত বিভিন্ন তথ্য বের করার পদ্ধতি দেখানো হয়েছেঃ

## SELinx User:

SELinux কনটেক্সট (Context) এর প্রথম অংশ হচ্ছে SELinux ইউজার পার্ট। অর্থাৎ, প্রথম অংশ দেখে ইউজার SELinux ইউজার আইডেন্টিফাই করা যাবে। RHEL সিস্টমে আট (০৮) ধরনের SELinux ইউজার থাকে। একজন SELinux ইউজার একাধিক SELinux রোল (Role) ম্যানেজ করতে পারে। নিচের কমান্ড ব্যবহার করে সিস্টেমে কি কি SELinux ইউজার আছে, এই সম্পর্কে জানা যাবে।

[root@node1 ~]# seinfo -u

Users: 8
sysadm_u
system_u
xguest_u
root
guest_u
staff_u
user_u
unconfined_u

## Role:

SELinux কনটেক্সট (Context) এর দ্বিতীয় অংশ হচ্ছে ‘Role’ এবং ‘Role’ সমূহ ‘\_r’ দিয়ে উল্লেখ করা হয়। একজন SELinux ইউজার একাধিক ‘Role’ ম্যানেজ করতে পারে। SELinux ইউজার কর্তৃক যে সকল 'Role' ম্যানেজ করা হয়, সেটা জানার জন্য নিচের কমান্ড।

[root@node1 ~]# seinfo -r
Roles: 15
auditadm_r
dbadm_r
guest_r
logadm_r
nx_server_r
object_r
secadm_r
staff_r
sysadm_r
system_r
unconfined_r
user_r
webadm_r
xguest_r

## SELinux Contexts: (-t)

সিস্টেমে কি কি security context (\_t) এবং মোট কতগুলা সিকিউরিটি Context আছে সেটা জানার জন্যঃ

[root@node1 ~]# seinfo -t  
[root@node1 ~]# seinfo -t | wc -l
4960

[root@node1 ~]# seinfo -t | grep sshd

[root@node1 ~]# yum install httpd -y

[root@node1 ~]# systemctl restart httpd
[root@node1 ~]# systemctl enable httpd
[root@node1 ~]# systemctl status httpd

[root@node1 ~]# firewall-cmd --permanent --add-port=80/tcp

[root@node1 ~]# firewall-cmd --reload
[root@node1 ~]# iptables -F

After changing ssh or other port - from eng tuts
[root@node1 ~]# firewall-cmd --zone=public --add-port=2224/tcp

## Create a webpage:

[root@node1 ~]# echo "HEllo World" >> /var/www/html/index.html

Visit web page: http://172.25.11.X

## SELinux Testing: Example: 01

[root@node1 ~]# cd
[root@node1 ~]# ls -Zd ~
dr-xr-x---. root root system_u:object_r:admin_home_t:s0 /root

[root@node1 ~]# touch redhat
[root@node1 ~]# ls -Z redhat
-rw-r--r--. root root unconfined_u:object_r:admin_home_t:s0 redhat

[root@node1 ~]# cp redhat /var/www/html/centos
[root@node1 ~]# mv redhat /var/www/html/
[root@node1 ~]# ls -Zd /var/www/html/
drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html/

[root@node1 ~]# ls -Z /var/www/html/\*
-rw-r--r--. root root unconfined_u:object_r:admin_home_t:s0 redhat
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 centos
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 index.html

## Change SELinux Security Context: (Method-1)

[root@node1 ~]# restorecon -v /var/www/html/redhat ; প্যারেন্ট ডিরেক্টরি অনুযায়ী ফাইলটি 'relabel' হবে।
[root@node1 ~]# ls -Z /var/www/html/redhat
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 redhat

## Method 02: (Context Change)

[root@node1 ~]# chcon -t httpd_sys_content_t /var/www/html/redhat
[root@node1 ~]# ls -lZ /var/www/html/redhat

[root@node1 ~]# semanage fcontext -l
[root@node1 ~]# semanage fcontext -l | grep "/var/www"

Note: # semanage fcontext -a -t httpd_sys_content_t '/var/www/html(/.\*)?' # restorecon -Rv

# SELinux Testing: Example: 02

[root@node1 ~]# mkdir /webhosting

[root@node1 ~]# ls -Zd /webhosting
drwxr-xr-x. root root unconfined_u:object_r:default_t:s0 /webhosting

[root@node1 ~]# touch /webhosting/index.html
[root@node1 ~]# echo "HEllo SELinux World" >> /webhosting/index.html

[root@node1 ~]# ls -Z /webhosting
-rw-r--r--. root root unconfined_u:object_r:default_t:s0 index.html

[root@node1 ~]# vim /etc/httpd/conf/httpd.conf

124 #DocumentRoot "/var/www/html"
123 DocumentRoot "/webhosting"
134 <Directory "/webhosting">

[root@node1 ~]# systemctl restart httpd

=> Switch to DesktopX
=> Open Firefox Browser
=> http://172.25.11.254

এখন সাইট ভিজিট করলে ডিফল্ট হোম পেজ আসবে, কারণ SELinux এখন Enforcing মোডে আছে এবং '/webhosting' ডিরেক্টরির সিকিউরিটি কন্টেক্সট (context) 'http' সার্ভিসের সাথে মিল নাই। তবে আমরা যদি এটাকে 'setenforce 0' কমান্ড দিয়ে 'Permissive' মোড সেট করে দিয়, তাহলে নতুন লোকেশন থেকে পেজ রান হবে।

=> http://172.25.11.254/index.html

এমতাবস্থায় SELinux 'Permissive' মোডে রাখলে ওয়েব পেজ আসবে।

[root@node1 ~]# setenforce 0
[root@node1 ~]# getenforce

=> http://172.25.11.254/index.html

[root@node1 ~]# setenforce 1
[root@node1 ~]# getenforce

=> http://172.25.11.254/index.html

কিন্ত, আমাদেরকে সব সময় SELinux মোড 'Enforcing' মোডে রেখে কাজ করতে হবে। নিচের কমান্ডের মাধ্যমে 'httpd_sys_content_t' কন্টেক্সট (context) দিয়ে '/webhosting' ডিরেক্টরি এবং এটার মধ্যে সকল সাব-ডিরেক্টরি, ফাইল সমূহের লেভেল পরিবর্তন করা হয়েছে।

[root@node1 ~]# semanage fcontext -a -t httpd_sys_content_t '/webhosting(/.\*)?'
[root@node1 ~]# restorecon -Rv /webhosting
[root@node1 ~]# getenforce

এখন যদি পুনরায় সাইট ভিজিট করা হয়, তাহলে ওয়েব সাইট দেখা যাবে।

=> http://172.25.11.254/index.html

## SELinux Port Labeling:

SELinux ফাইল এবং প্রসেস লেভেলে সিকিউরিটি (SELinux Policy) দেওয়ার পাশাপাশি নেটওয়ার্ক ট্রাফিকেও খুব শক্তভাবে মনিটরিং এবং সিকিউরিটি প্রদান করে। যেমন, বিভিন্ন ধরনের নেটওয়ার্ক সার্ভিস সমূহ (TCP/UDP) SELinux সিকিউরিটি কর্তৃক নিয়ন্ত্রণ করা হয়। উদাহরণ স্বরূপ SSH সার্ভিসের ডিফল্ট পোর্ট হিসেবে 22/TCP, HTTP সার্ভিসের জন্য '80' এবং HTTPs সার্ভিসের জন্য ডিফল্ট পোর্ট '443' ইত্যাদি SELinux কর্তৃক লেভেল (bind) করা থাকে। যেমনঃ

- ssh_port_t = 22/TCP
- http_port_t = 80/TCP & 443/TCP

যখন সিস্টেমে বিভিন্ন সার্ভিসের ডিফল্ট পোর্ট পরিবর্তন করে যদি অন্য পোর্ট ব্যবহার করা হয়, তখন SELinux সিকিউরিটি কর্তৃক সেই সার্ভিস বন্ধ করে দেওয় হয়।

[root@node2 ~]# netstat -ntlp | grep ssh ; বর্তমানে 'SSH' সার্ভিস কোন পোর্টে রান করছে সেটা দেখার জন্য।

[root@node2 ~]# semanage port -l ; SELinux কর্তৃক লেভেলকৃত পোর্টে সমূহের লিস্ট দেখা যাবে।  
[root@node2 ~]# semanage port -l | grep ssh ;SELinux কর্তৃক লেভেলকৃত 'SSH' সার্ভিসের পোর্ট নাম্বার দেখার জন্য।  
 ssh_port_t tcp 22

[root@node2 ~]# semanage port -l | grep http

[root@node2 ~]# semanage port -a -t ssh_port_t -p tcp 2222 ;'SSH' এর পোর্ট '22' এর পাশাপাশি '2222' পোর্টেও লেভেল (bind) করা হল।

[root@node2 ~]# semanage port -l | grep ssh
ssh_port_t tcp 2222, 22

\*\* Change SSH Default port '2222'

## SELinux Boolean:

SELinux Booleans এক ধরনের কনফিগারেশন পদ্ধতি যেটা ব্যবহার করে অ্যাডমিনিস্ট্রেটর খুব সহজে SELinux পলিসি পরিবর্তন ছাড়াই ব্যবহার করতে পারবে।

[root@node2 ~]# getsebool -a ; সিস্টেমে যত 'Boolean' আছে সকল লিস্ট দেখার জন্য।

[root@node2 ~]# semanage boolean -l
[root@node2 ~]# semanage boolean -l | grep ftp ; service must be enabled
[root@node2 ~]# semanage boolean -l | grep httpd ; service must be enabled

[root@node2 ~]# semanage boolean -l -C

যে কোনো Boolean এনাবেল করার জন্য 'on' বা ডিজাবেল করার জন্য 'off' করতে হবে।

## Boolean Enable করার জন্য নিচের কমান্ডঃ

[root@node2 ~]# setsebool -P < boolean_name > on

## Lab Demo:

এখানে 'student' ইউজারের 'home' ডিরেক্টরিতে একটি webpage হোস্ট করা হবে। উক্ত ওয়েব পেজটির হোস্টিং ডিরেক্টরি 'SELinux Boolean' দিয়ে enable
করতে হবে।

[root@node2 ~]# vim /etc/httpd/conf.d/userdir.conf

11 <IfModule mod_userdir.c>
17 # UserDir disabled
24 UserDir public_html
25 </IfModule>

[root@node2 ~]# systemctl restart httpd
[root@node2 ~]# su - student
[student@node2 ~]$ mkdir ~/public_html
[student@node2 ~]$ echo 'This is student content on Node1' > ~/public_html/index.html

[student@node2 ~]$ chmod 711 ~

## Student ইউজারের হোস্টিং এক্সেস করার জন্য নিচের URL অনুযায়ী চেক করতে হবেঃ

http://172.25.11.254/~student/index.html ভিজিট করতে হবে।

Output: An error message states that you do not have permission to access the file.

[root@node2 ~]# semanage boolean -l | grep httpd_enable_homedirs
httpd_enable_homedirs (off , off) Allow httpd to read home directories

Enable Boolean 'httpd_enable_homedirs' for user home directory:

---

[root@node2 ~]# setsebool -P httpd_enable_homedirs on

[root@node2 ~]# getsebool httpd_enable_homedirs
httpd_enable_homedirs --> on

Now Visit: http://172.25.11.254/~student/index.html ভিজিট করতে হবে।

====================== Thank you ======================

## Homework:

Your company has decided to run a new web app. This application listens on ports 80/TCP and
8090/TCP. Port 2222/TCP for ssh access must also be available. All changes you make should
persist across a reboot.

# sealert -a /var/log/audit/audit.log
