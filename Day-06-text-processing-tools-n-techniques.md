# Day 06: Linux Text Processing Tools & Techniques

`echo, cat, less, head, tail, grep, wc, locate, find, ,man, help`

ল্যাব প্রাক্টিসের জন্য আমরা একটা ডিরেক্টরি তৈরি করবো `mkdir -p /linux/day6` নামে। এবং `cd /linux/day6` কমান্ড দিয়ে `day6` ডিরেক্টরিতে প্রবেশ করবো। আজকের ল্যাব প্র্যাকটিস করার জন্য `passwd` ফাইলটি কপি করে `cp /etc/passwd .` বর্তমান ডিরেক্টরির মধ্যে নিয়ে আসাব।

## Working with cat command

- `cat passwd` ফাইলের কনটেন্ট দেখার জন্য `cat` কমান্ড ব্যবহার করা হয়।
- `cat -n passwd` লাইন নাম্বার সহ ফাইল দেখার জন্য `-n` অপশন ব্যবহার করতে হবে।
- `cat /etc/hostname` অন্য কোনো ডিরেক্টরির ভিতরের ফাইল দেখার জন্য।
- `cat /etc/redhat-release` RHEL কোন ভার্শন ব্যবহার করা হচ্ছে, সেটা জানার জন্য।

## Working with ech' and variables

- `echo` কমান্ড ব্যবহার করে টার্মিনালে যে কোনো টেক্সট প্রিন্ট করা যাবে। পাশাপাশি বিভিন্ন ধরনের সিস্টেম ভেরিয়েবল, ইউজার ডিফাইনড ভেরিয়েবল এবং চাইলে সিস্টেমের যেকোনো কমান্ডকেও কল করা যাবে।

- `clear` স্ক্রিন ক্লিয়ার করার কমান্ড।
- `Hello World` আমরা যদি লিনাক্স টার্মিনালে `Hello World` লিখে এন্টার প্রেস করলে `Bash Error` আসবে।
- `echo "Hello World"` কিন্ত আমরা যদি `Hello World` কে `echo` কমান্ডের মাধ্যমে কল করি, তাহলে সেটা দেখা যাবে।
- `echo $SHELL` লিনাক্স টার্মিনালে ব্যবহৃত শেল/ইন্টারপ্রিটার সম্পর্কে জানার জন্য। এখানে `SHELL` বড় হাতের অক্ষরে হবে।
- `echo $HOSTNAME` লিনাক্স সিস্টেমে বিল্ট-ইন `Environment Variable` বড় হাতের (Block Letter) অক্ষরে প্রকাশ করা হয়।
- `a=100` user defined variable
- `echo $a`
- `os=linux`
- `echo $os`
- `date`
- `echo "Today is: $(date)"` all bash command

## Working with Input and Output Redirection (>, >>)

লিনাক্স `CLI` তে তিন ধরনের ডেটা স্ট্রিম আছে ইনপুট/আউট

- `stdin` standard input (Value 0) - এক্ষেত্রে টেক্সট ইনপুট হিসেবে গ্রহন করে। stdin ডিভাইস হচ্ছে Keyboard
- `stdout` standard output (Value 1) - Stdout ডিভাইস হচ্ছে মনিটর
- `stderr` standard error (Value 2) - Stderr ডিভাইস error আউপুট জমা রাখে। কমান্ডে যখন কোনো এরর আসে, তখন এরর গুলো জমা করা হয়।

## Output Redirection

- `echo "Welcome to RHEL 9"`
- `echo "Welcome to RHEL 9" > file1`
- `cat file1`
- `echo "Welcome to CentOS 9" > file1` Overwrite করবে।
- `cat file1`
- `echo "Welcome to RHEL 9" >> file1` append
- `cat file1`
- `history`
- `history > shell_history`
- `cat shell_history`
- `ping -c4 192.168.0.106 > ping`
- `ls`
- `cat ping`

নোটঃ আউটপুট রিডিরেকশনে ফাইলের নাম দিলেই তৈরি হয়ে যাবে, আগে থেকে থাকা জরুরি নয়। যদি থাকে তাতেও সমস্যা নাই।

## Working with head, tail & less command

`cat` কমান্ড স্ক্রিনে যতটুকু কনটেন্ট ধরবে শুধু ততটুকুই দেখাবে যদিও ফাইলে অনেক বেশি কনটেন্ট থাকুক না কেন। সব কনটেন্ট স্ক্রল করে দেখার জন্য আমরা `less` কমান্ড ব্যাবহার করতে পারি। `up` এবং `down` অ্যারো প্রেস করে স্ক্রল করতে পারি।

- `less passwd` ফাইল উপর থেকে নিচ পর্যন্ত স্ক্রলিং করে দেখা যাবে। `Press q to quit`
- `head passwd` ফাইলের প্রথম ১০ লাইন দেখা যাবে।
- `tail passwd` ফাইলের শেষের ১০ লাইন দেখা যাবে।
- `tail -15 passwd` ফাইলের শেষের ১৫ লাইন দেখা যাবে।
- `head -5 passwd` ফাইলের প্রথম ৫ লাইন দেখা যাবে।

## Pipeline

`|` Pipeline মূলত ব্যবহার করা হয়, এক কমান্ডের আউটপুট ফিল্টার করে আরেকটি আউটপুট পাওয়ার জন্য। কমান্ডের সাথে একই সময়ে একাধিক বার `|` Pipeline ব্যবহার করা যাবে।

- `cat -n passwd | head`
- `cat -n passwd | tail`
- `cat -n passwd | less` press 'q' to quit
- `cat -n passwd | tail -15 | head -5` লাস্টের ১৫ লাইনের প্রথম ৫ লাইন দেখা যাবে।
- `cat -n passwd | head -15 | tail -5` প্রথম ১৫ লাইনের শেষের ৫ লাইন দেখা যাবে।
- `ifconfig | grep ether`

## Working with grep command

`grep` এর কাজ হচ্ছে ফাইলের ভিতর থেকে বা কোনো কমান্ডের আউটপুট থেকে কীওয়ার্ড খুঁজে বের করা। `grep` কমান্ড ব্যবহার করে ফাইলের মধ্যে, লাইনের শুরুতে, লাইনের শেষে কোন কিছু খুঁজতে পারি। `grep` কে অন্য কমান্ড `cat, tail, head, less` এর সাথেও ব্যবহার করতে পারি।

- `grep -n root passwd` এখানে `root` কীওয়ার্ডটি `passwd` ফাইলে খোজা হচ্ছে।

নোটঃ আউটপুটে দেখা যাচ্ছে `root` কীওয়ার্ড আছে এবং `-n` অপশন ব্যবহার করা হয়েছে লাইন নাম্বার দেখার জন্য। এখানে `passwd` ফাইলের ১ এবং ১০ নাম্বার লাইনে `root` কীওয়ার্ডটি আছে।

- `grep -n ^root passwd` লাইনের শুরুতে `root` দেখার জন্য।
- `grep FTP passwd`
- `grep "FTP User" passwd` একাধিক কীওয়ার্ড (String) একসাথে খুঁজতে চাইলে ডাবল কোট ("") ব্যবহার করতে হবে।
- `grep -i ftp passwd` ছোট হাতের বড় হাতের সব ধরণের ক্যারেক্টার খুজতে চাইলে।
- `tail passwd | grep root` আমরা চাইলে `tail` কমান্ডের সাথেও `grep` ব্যবহার করতে পারি।
- `head passwd | grep root` আমরা চাইলে `head` কমান্ডের সাথেও `grep` ব্যবহার করতে পারি।
- `cat passwd | grep bash` আমরা চাইলে `cat` কমান্ডের সাথেও `grep` ব্যবহার করতে পারি।

## Working with wc command

কোনো ফাইলের মধ্যে কতগুলা লাইন, ওয়ার্ড, ক্যারেক্টার (Letter) আছে সেটা দেখাবে। কোনো ডিরেক্টরির মধ্যে কতগুলা ফাইল/ডিরেক্টরি আছে সেই লিস্টও পাওয়া যাবে।

- `wc passwd`
- `ls | wc -l`
- `ls /etc | wc -l`

## Working with Linux locate command

`locate` কমান্ডের মাধ্যমে লিনাক্স সিস্টেমের মধ্যে যে কোনো ফাইল/ডিরেক্টরি খুঁজে বের করা যাবে। `locate` কমান্ড প্র্যাকটিস করার জন্য প্রথমে `root` ইউজারের প্রিভেলেজ নিতে হবে। এক্ষেত্রে নিচের `su` (switch user) কমান্ড ব্যবহার করতে হবে।

- `updatedb` লিনাক্স সকল ফাইল/ডিরেক্টরি আপডেট `Cache` করার কমান্ড।
- `locate sshd_config` যে কোন লোকেশনে `sshd_config` ফাইলে খুঁজে বের করার জন্য।
- `locate -i .jpg` (i=insensitive) word সাথে extension সহ খুঁজে বের করার জন্য।
- `locate .jpg | grep sky`
- `locate Dhaka`

## Working with Linux find command

- `find / -name mail` mail নামে যে কোনো ফাইল/ফোল্ডার রুট (/) ফাইল সিস্টেমে খোঁজার জন্য।
- `find /var -name mail` নির্দিষ্ট লোকেশনে '/var' মধ্যে 'mail' নামে কিছু খোঁজার জন্য।
- `find / -user student` student নামে Owner নামে যত ফাইল আছে, সব খুঁজে বের করার জন্য।
- `ls -l file_path` বুঝার জন্য একটা ফাইল পাথ কপি করে `ls` করে দেখতে পারি।
- `find / -type d -name ssh` ssh নামে ডিরেক্টরি খুঁজে বের করার জন্য।
- `find / -type f -name ssh` ssh নামে ফাইল খুঁজে বের করার জন্য।
- `find / -type b` সিস্টেমের সকল ব্লক ডিভাইস ফাইল `b` খুঁজে বের করার কমান্ড।
- `find / -size +100M` 100 MB এর উপরের সাইজের সকল ফাইল খুঁজে বের করার জন্য।
- `find / -size +100M | wc -l > file_name`
- `find / -size +100M | grep rpm`
- `ls -lh /usr/lib64/firefox/libxul.so`
- `ls -sh /usr/lib64/firefox/libxul.so`

## Working with Help and Manual

- `man`
- `man ping` ping সম্পর্কিত সকল কমান্ডের তথ্য পাওয়া যাবে। বের হওয়ার জন্য 'q' প্রেস করতে হবে।
- `ping --help`
- `ping -?`
- `useradd -?`
- `mandb` ম্যান কমান্ড কাজ না করলে
- `whatis sshd_config` সিস্টেমের নির্দিষ্ট একটি ফাইল সম্পর্কে জানার জন্য।
- `whatis ping` নির্দিষ্ট কমান্ড `ping` সম্পর্কে জানার জন্য।
- `whatis find` নির্দিষ্ট কমান্ড `find` সম্পর্কে জানার জন্য।

`file:////usr/share/doc`

## The Info Command

`info ping` স্পেসবার ব্যবহার করে সামনের দিকে যাওয়া যাবে। বের হওয়ার জন্য `q` প্রেস করতে হবে।

## Online Documentation

docs.redhat.com -> RedHat Enterprise Linux -> System Administration -> PDF

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9

## Home work

Find out and store number of log messages generated by your system on "Jun 5".

Log location `less /var/log/messages`
