# Class 19:

নিচের কমান্ড দিয়ে আমরা শুধুমাত্র `/etc` ডিরেক্টরিতে সকল হিডেন ফাইলগুলো খুজতে পারি। যেহেতু হিডেন ফাইল শুরু হয় (.) dot দিয়ে।
`find /etc -type f -iname ".*"`

নিচের কমান্ড দিয়ে আমরা শুধুমাত্র `/etc` ডিরেক্টরিতে হিডেন ফাইল ছাডা বাকি সকল ফাইল আউটপুটে দেখাবে।
`find /etc -type f ! -iname ".*"`

একই আউটপুট আমরা এভাবেও পেতে পারি।
`find /etc -type f -not -iname ".*"`

নির্দিষ্ট কোন ফাইল টাইপ ছাড়া অন্য ফাইল খুজতে চাইলে।
`find /etc -type (-not/!) f -iname ".*"`

### Search by File Size

1MB এর উপরে ফাইল সার্চ করার জন্য।
`find /etc -type f -iname "*" -size +1M`

`1M` এক্স্যাক্ট 1MB

`+1M` 1MB এর উপরে

`-1M` 1MB এর নিচে।

ফাইল সাইজ চেক করার জন্য

`du -sh /etc/udev/hwdb.bin`

`du -sh file_name_with_path`

Exact match এর জন্য

`find /etc -type f -iname "*" -size 12M -exec du -sh {} \;`

`find /etc -type f -iname "*" -size +2M -size -13M -exec du -sh {} \;`

2MB এর নিচের ফাইল সার্চ করে নিয়ে আসবে।

`find /etc -type f -iname "*" -size -2M -exec du -sh {} \;`

ফাইলের সাইজের রেঞ্জ দিয়ে সার্চ করার জন্য।

`find /etc -type f -iname "*" -size +3M -size -13M`

### Sorting

Ascending Order অনুযায়ী সার্চ রেজাল্ট দেখার জন্য।

`find /etc -type f -iname "*" -size -2M -exec du -sh {} \; | sort`

Numerically Assending order অনুযায়ী সার্চ।

`find /etc -type f -iname "*" -size -2M -exec du -sh {} \; | sort -n`

`Study more about awk, sed, & cut command`

### Size allocation for file

`fallocate -l 1.5G /opt/file_name.txt`

`-l for file length or size`

আমরা ৫ টা ফাইল তৈরি করি `/opt/` ডিরেক্টরিতে।

`fallocate -l 1G /opt/1G.txt`
`fallocate -l 1.5G /opt/1.5G.txt`
`fallocate -l 2G /opt/2G.txt`
`fallocate -l 2.5G /opt/2.5G.txt`
`fallocate -l 3G /opt/3G.txt`

`find /opt -type f -iname "*" -size +1G -size -3G -exec du -sh {} \;`

এই কমান্ডে 2.5G শো করবে না। যদি পেতে চাই তাহলে GB কে মেগা বাইটে কানভারট করে দিতে হবে। যেমনঃ

`find /opt -type f -iname "*" -size +1024 -size -3072M -exec du -sh {} \;`

inode নাম্বার দিয়ে সার্চ করা।

`find / -type f -iname "*" -inum inode_number`

যে ফাইলে লিঙ্ক করা আছে এমন ফাইলের নাম দিয়েও সার্চ করা যায়।

`find / -type f -iname "*" -samefile file_path`

`find / -type f -iname "*" -atime`

`find / -type f -iname "*" -atime` Access Time

`find / -type f -iname "*" -mtime` Modification Time (Day wise)

`find / -type f -iname "*" -ctime` Change Time (Permission)

`find / -type f -iname "*" -amin` Access Minute

`find / -type f -iname "*" -mmin` Modification Minute (Day Wise)

`find / -type f -iname "*" -cmin` Change Minute (Permission)

`find / -maxdepth 2 -type f -iname "*" -mmin -2` ২ মিনিটের ভিতরে যেই ফাইলগুলো মডিফাই করা হয়েছে।

`find / -maxdepth 2 -type f -iname "*" -cmin -2` ২ মিনিটের ভিতরে যেই ফাইলগুলোর পারমিশন চেঞ্জ করা হয়েছে।

`find / -maxdepth 2 -type f -iname "*" -cmin +2 -cmin -10` ২ মিনিটের উপরে এবং ১০ মিনিটের মধ্যে যেই ফাইলগুলোর পারমিশন চেঞ্জ করা হয়েছে।

`find / -type f -name "*" -user munir` মুনির ইউজারের ওনারশিপে যেই ফাইলগুলো আছে সেইগুলো সার্চ করবে।

### Exam Question

`find / -type f -name "*" -exec cp -vfp {} /destination/ \;`

নির্দিষ্ট কোন ইউজারের ফাইল খুঁজে বের করে সেই ফাইলগুলো কপি করে অন্য লোকেশনে আরেক ডিরেক্টরিতে পেস্ট করার জন্য। কমান্ড রান করার আগে চেক করে নেয়া যে ফাইল এবং ডেসটিনেশন ডিরেক্টরি এক্সিস্ট করে কিনা।

`find / -type f -name "*" -user munir -exec cp -vfp {} /destination/ \;`

`-f for Forcefully`

`-v for Verbose`

`-p for Preserve time` কপি করার সময় মডিফাই টাইমে কারেন্ট টাইম চলে আসে। অতএব ফাইল/ডিরেক্টরির মডিফাই টাইম পরিবর্তন করতে না চাইলে `-p` অপশন ইউজ করতে পারি।

`{}` রান টাইমে টেম্পরারি স্টোর করার জন্য ইউজ করতে হয়।

### Content Search

`grep "root" /etc/passwd`

`grep -i "root" /etc/passwd` For ignore case

`grep -in "root" /etc/passwd` For ignore case and to show line number.

`grep -v "root" /etc/passwd` যেই লাইনে `root` আছে সেই লাইনগুলো বাদে বাকিগুলো সার্চ করার জন্য।

`grep -v "#" /etc/fstab` যেই লাইনে `#` আছে সেই লাইনগুলো বাদে বাকিগুলো সার্চ করার জন্য।

`-v for inverter`

`grep -c "root" /etc/passwd` মোট কতগুলো লাইন আছে তা দেখার জন্য।

`egrep for Expression grep`

`grep -i -e "root" -e "nologin" -e "bash" /etc/passwd` একসাথে একাধিক ওয়ার্ড সার্চ করার জন্য।

`-e for Expression`

### Conditional Statement

`&&` `and Operator` ২ টা কন্ডিশনই সঠিক হতে হবে। যেমনঃ `pwd && whoami` এক্ষেত্রে `pwd` কমান্ড সঠিক হলে `whoami` কমান্ডটি রান করবে। আর প্রথম কমান্ডটি সঠিক না হলে দ্বিতীয় কমান্ডটি রান করবে না।

`||` `or Operator` ২ টার যেকোনো একটি কন্ডিশন সঠিক হলেই কমান্ডটি রান করবে। যেমনঃ `adfadf || whoami` এক্ষেত্রে প্রথম কমান্ডটি সঠিক না হলেও দ্বিতীয় কমান্ডটি রান করবে।

`|` `Separator` ২ বা ততোধিক কমান্ডকে আলাদা বুঝানোর জন্য ব্যাবহার করা হয়।

`grep -inE "Root|sync" /etc/passwd` এভাবে আমারা একাধিক কিওয়ারড সার্চ করতে পারি।

`grep -inA 2 "Root" /etc/passwd` যেই লাইনে কিওয়ারডটি ম্যাচ করবে সেই লাইন সহ পরের ২ লাইন দেখার জন্য।

`grep -inB 2 "Root" /etc/passwd` যেই লাইনে কিওয়ারডটি ম্যাচ করবে সেই লাইন সহ আগের ২ লাইন দেখার জন্য।

`grep -inC 2 "Root" /etc/passwd` যেই লাইনে কিওয়ারডটি ম্যাচ করবে সেই লাইন সহ সেই লাইনের আগে এবং পরে ২ লাইন দেখার জন্য।

`A for After`

`B for Before`

`C for Center`

`grep -in "^Root" /etc/passwd` যদি প্রথম ওয়ার্ডটি ম্যাচ হয় তাহলে সেটিই দেখাবে। কিবোর্ডের উপরের সারির নিচের সারিতে নাম্বার সিক্স বাটন। (Shift + 6)

`grep -in "bash$" /etc/passwd` যেই সকল লাইনের শেষে `bash` ওয়ার্ড থাকবে শুধুমাত্র সেই লাইনগুলো দেখাবে।

`$` দিয়ে লাইনের শেষের অংশ বা ওয়ার্ড বুঝায়।

`grep -inE "^root|bash$" /etc/passwd` লাইনের শুরু এবং/অথবা শেষে ম্যাচ করলে।

`grep -in "^[a-f]" /etc/passwd` লাইনের শুরুতে a থেকে f লেটার পরযুন্তু ম্যাচ করলে রেজাল্ট দেখাবে।

`grep -inE "^[s-z]|[a-f]$" /etc/passwd`

### Expression grep

`egrep -in "root|bash" /etc/passwd`

`-E for Extended Expression`

Regular expression সম্পর্কে আইডিয়া থাকা ভাল।
[Learn more about RegEx](https://www.youtube.com/watch?v=rhzKDrUiJVk)

(Optional Learning) awk & sed command
