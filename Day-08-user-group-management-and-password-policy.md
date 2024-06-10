# Day 08: User and Group Management

## Module Objectives

- লিনাক্স সিস্টেমে বিভিন্ন ধরনের ইউজার এবং গ্রুপ সম্পর্কে বিস্তারিত আলোচনা।
- লিনাক্স সিস্টেমের ইউজার প্রোফাইল এবং গ্রুপ প্রোফাইল নিয়ে বিস্তারিত আলোচনা।
- লিনাক্স সিস্টেমের ইউজার এবং গ্রুপ ম্যানেজমেন্টের উপরে বিস্তারিত আলোচনা।
- লিনাক্সে `Superuser` এবং `SUDO` ইউজারের প্রিভেলেজ সম্পর্কে বিস্তারিত আলোচনা।
- লিনাক্স সিস্টেমে ইউজারের পাসওয়ার্ড প্রোফাইল এবং পাসওয়ার্ড পলিসি নিয়ে বিস্তারিত আলোচনা।

লিনাক্স সিস্টেমে সব ধরনের ইউজারদের ডাটাবেজ ফাইল হচ্ছে `/etc/passwd` নিচের কমান্ডের মাধ্যমে সিস্টেমের সকল ইউজার সম্পর্কিত তথ্য পাওয়া যাবে।

`less /etc/passwd` স্ক্রলিং করে সম্পূর্ণ ফাইল দেখা যাবে। বের হওয়ার জন্য কীবোর্ড থেকে `q` প্রেস করতে হবে।

## লিনাক্স সিস্টেমে তিন (০৩) ধরনের ইউজার আছে।

- 1. root : 0
- 2. system user: 1 - 999
- 3. regular user: 1000 +

এখানে `root` ইউজারের আইডি (UID) `0`, সিস্টেম ইউজারের আইডি (UID) `1-999` এবং রেগুলার ইউজারের আইডি (UID) `1000` থেকে শুরু হয়। লিনাক্স সিস্টেম ডিফল্ট ভাবে `60,000` ইউজার সাপোর্ট করে। তবে বেশি ইউজার হলে লোকাল সিস্টেমের বদলে ডাটাবেজ ব্যবহার হয়। কারণ, বেশি ইউজার লোকাল সিস্টেম দিয়ে ম্যানেজ করা কঠিন হয়ে যায়।

নির্দিষ্ট কোনো ইউজারের ইউজার আইডি (UID) জানার জন্য নিচের কমান্ড

- `id root` root ইউজার সিস্টেমে আছে কিনা জানার জন্য।
- `id mail` mail ইউজার সিস্টেমে আছে কিনা জানার জন্য।
- `id student` student ইউজার সিস্টেমে আছে কিনা জানার জন্য।
- `id munir` munir ইউজার সিস্টেমে আছে কিনা জানার জন্য।
- `useradd munir` নতুন ইউজার হিসেবে `munir` ইউজারকে তৈরি করা হয়েছে।
- `id munir` munir ইউজার তৈরি হয়েছে কিনা সেটা দেখার জন্য।
- `tail /etc/passwd` `/etc/passwd` ফাইলে `munir` ইউজারের প্রোফাইল যোগ হয়েছে কিনা সেটা দেখার জন্য।
- `grep munir /etc/passwd` `/etc/passwd` ফাইল থেকে শুধুমাত্র `munir` ইউজারের প্রোফাইল চেক করার জন্য।

```
munir:x:1000:1000:Munir Mahmud:/home/munir:/bin/bash
  1   2   3    4       5            6         7
```

- 1. ইউজারের লগইন নাম, যেটা ইউনিক হবে এবং নামের মাঝে কোনো স্পেস থাকবে না। যেমনঃ munir, munir.khan, munir123, munir_123
- 2. ইউজারের পাসওয়ার্ড সম্পর্কিত তথ্য থাকে যেটা মূলত থাকে `/etc/shadow`
- 3. ইউজার আইডি `UID` প্রত্যেকটি ইউজারের আলাদা আলাদা ইউজার আইডি `UID` থাকবে।
- 4. প্রাইমারী গ্রুপ আইডি `GID`, ইউজার আইডি `UID` এর পাশাপাশি প্রত্যেকটি ইউজারের `GID` থাকবে, এটাকে বলা হয়, প্রাইমারী গ্রুপ `GID` আইডি।
- 5. ইউজারের পূর্ণ নাম বা বিস্তারিত তথ্য। যেমনঃ `Munir Mahmud, Database Admin, System Admin`
- 6. ইউজারের হোম ডিরেক্টরি। যখন কোনো রেগুলার ইউজার তৈরি করা হয়, তখন `/home` ডিরেক্টরির মধ্যে সেই ইউজারের নামে একটি ডিরেক্টরি তৈরি হয়। যার মধ্যে উক্ত ইউজারের সকল প্রোফাইল সম্পর্কিত তথ্য থাকে।
- 7. ইউজারের ব্যবহৃত শেলের নাম `/bin/bash`। এটা শুধুমাত্র যে সকল ইউজার সিস্টেমে লগইন করবে তাদের ক্ষেত্রে থাকবে। যে সকল ইউজার সিস্টেমে লগইন করার পার্মিশন নাই তাদের ক্ষেত্রে `/sbin/nologin` থাকবে। ইউজারের শেল এক্টিভ/এনাবেল না থাকলে সেই ইউজার সিস্টেমে লগইন করতে পারবে না।

নোটঃ সিস্টেমের সকল ইউজারের পাসওয়ার্ড সম্পর্কিত তথ্য থাকে `/etc/shadow` ফাইলে। ইউজারের পাসওয়ার্ড সম্পর্কিত তথ্য জানার জন্য নিচের কমান্ড।

`tail /etc/shadow` `/etc/shadow` ফাইল থেকে `munir` ইউজারের পাসওয়ার্ড সম্পর্কিত তথ্য জানার জন্য নিচের কমান্ড।

- `grep munir /etc/shadow`
- `useradd hasan` `hasan` নামে আরও একজন ইউজার তৈরি করা হল।
- `useradd mamun` `mamun` নামে আরও একজন ইউজার তৈরি করা হল।
- `passwd hasan` `passwd` কমান্ডের মাধ্যমে `hasan` ইউজারের পাসওয়ার্ড সেট করা হয়েছে।

নোটঃ পাসওয়ার্ড দেওয়া এবং পরিবর্তন করার জন্য একই কমান্ড। পাসওয়ার্ড দেওয়ার সময় কিছু ওয়ার্নিং দিবে, পাসওয়ার্ড হিসেবে আমরা এখানে `linux` ব্যবহার করবো। পাসওয়ার্ড স্ক্রিনে দেখা যাবে না, সেই সাথে কোনো ইউজারের পুরাতন পাসওয়ার্ড দেখা যাবে না, বা জানা যাবে না। পাসওয়ার্ড এনক্রিপ্টেড `hash` ফরম্যাটে থাকে।

`tail /etc/shadow` এই ফাইলে `hasan` ইউজারের পাসওয়ার্ড সেট হয়েছে কিনা সেটা চেক জন্য।

যেহেতু আমরা `hasan` ইউজারের পাসওয়ার্ড সেট করেছি, সুতরাং আলাদা টার্মিনাল ব্যবহার করে লগইন করে টেস্ট করবো যে, `hasan` ইউজার দিয়ে সিস্টেমে লগইন করা যায় কিনা। আমরা কীবোর্ড থেকে `Alt + Ctl + F3` প্রেস করলে নতুন `Login Prompt` আসবে সেখানে `Login Name` হিসেবে `hasan` লিখে `Enter` প্রেস করলে `hasan` ইউজারের পাসওয়ার্ড দেওয়ার অপশন আসবে। এখন পাসওয়ার্ড হিসেবে `linux` টাইপ করে এন্টার প্রেস করলে লগইন হবে।

`exit` `hasan` ইউজার দিয়ে লগইন করলে, তার জন্য আলদা `shell` ওপেন হবে। এখান থেকে বের হওয়ার জন্য `exit` প্রেস করতে হবে। পুনরায় গ্রাফিক্যাল ইউজার ইন্টারফেসে ফিরে যাওয়ার জন্য `Alt + ctrl + F2` প্রেস করতে হবে।

## Home Work

Create an user `rony` and set his password `linux` and login through command console `tty5`.

## Linux Group Management

লিনাক্স সিস্টেমে নতুন `Group` তৈরি করার জন্য `groupadd` কমান্ড ব্যবহার করা হয়।

- `groupadd trainer` `groupadd` কমান্ড ব্যবহার করে `trainer` নামে একটি গ্রুপ তৈরি করা হয়েছে।
- `groupadd staff` `groupadd` কমান্ড ব্যবহার করে `staff` নামে আরও একটি গ্রুপ তৈরি করা হয়েছে।

নোটঃ যখন কোনো `Group` তৈরি করা হয় তখন `/etc/group` ফাইলে জমা হয়। লিনাক্স সিস্টেমে `Group` সম্পর্কিত তথ্য জানার জন্য নিচের কমান্ড।

`tail /etc/group`

```
trainer :x:  1002 :
   1     2     3    4
```

- 1. গ্রুপের নাম (প্রাইমারি/সেকেন্ডারি গ্রুপ)
- 2. গ্রুপের পাসওয়ার্ড সম্পর্কিত তথ্য থাকে যেটা মুলত থাকে `/etc/gshadow`
- 3. গ্রুপ আইডি `GID`
- 4. গ্রুপের সদস্য `Members`

নোটঃ যখন কোনো ইউজার তৈরি করা হয়, তখন ইউজার একাউন্টের পাশাপাশি উক্ত ইউজারের `/etc/group` ফাইলে গ্রুপ একাউন্টও তৈরি হয়। এটাকে বলা হয় প্রাইমারী গ্রুপ এবং প্রাইমারী গ্রুপ কখনো ডিলিট করা যায় না।

- `grep trainer /etc/group` `grep` কমান্ড ব্যবহার করে সিস্টেমে `trainer` Group আছে কিনা চেক করা হয়েছে।
- `useradd -G trainer pavel` নতুন ইউজার হিসেবে `pavel` ইউজারকে `trainer` গ্রুপে যোগ করা হল।
- `usermod -G trainer munir` পুরাতন ইউজার হিসেবে `munir` ইউজারকে `trainer` গ্রুপে যোগ করা হল।
- `grep trainer /etc/group` `grep` কমান্ড ব্যবহার করে সিস্টেমে `trainer` গ্রুপের মেম্বার লিস্ট দেখা হয়েছে।
- `useradd fabiha` `fabiha` নামে আরও একজন ইউজার তৈরি করা হল।
- `usermod -G trainer,staff fabiha` উপরের কমান্ডে `fabiha` ইউজারকে একাধিক গ্রুপে `staff & trainer` এ্যাড করা হল।
- `grep staff /etc/group` `staff` গ্রুপের তথ্য দেখার জন্য।
- `grep trainer /etc/group` `trainer` গ্রুপের তথ্য দেখার জন্য।
- `id fabiha` `fabiha` ইউজার কোন কোন গ্রুপের সদস্য সেটা দেখার জন্য ব্যবহার করা হয়েছে।
- `gpasswd -d fabiha staff` কোনো ইউজার কে নির্দিষ্ট কোনো গ্রুপ থেকে বাদ দেওয়ার জন্য `gpasswd` কমান্ড।
- `grep staff /etc/group` ইউজার `fabiha` `staff` গ্রুপ থেকে রিমুভ হয়েছে কিনা সেটা চেক করার জন্য।

## User Profile Modification

Login as root user

- `useradd -u 3000 rafat` নির্দিষ্ট ইউজার আইডি `UID` ব্যবহার করে `rafat` নামে ইউজার তৈরি করা হয়েছে।
- `id rafat` `rafat` ইউজার তৈরি হয়েছে কিনা সেটা জানার জন্য।
- `groupadd -g 4000 support` নির্দিষ্ট গ্রুপ আইডি `GID` ব্যবহার করে `support` নামে গ্রুপ তৈরি করা হয়েছে।
- `tail /etc/group` গ্রুপ সম্পর্কিত তথ্য দেখার জন্য।
- `grep rafat /etc/passwd` `rafat` ইউজারের প্রোফাইল চেক করা হয়েছে।

এখন আমরা `rafat` ইউজারের প্রোফাইল পরিবর্তন করবো। এখানে আমরা `rafat` ইউজারের পূর্ণ নাম, হোম ডিরেক্টরি এবং শেল পরিবর্তন করবো।

- `usermod -c "Mr. Rafat Hussain" rafat  `

নোটঃ `rafat` ইউজারের পূর্ণ নাম যোগ করা হয়েছে। এখানে `-c` অপশন ব্যবহার করা হয়েছে যার অর্থ `comment`।

- `grep rafat /etc/passwd` এই কম্যান্ড রান করলে আমরা দেখতে পাবো যে, `rafat` ইউজারের পূর্ণ নাম এ্যাড হয়েছে।

যখন কোনো ইউজার তৈরি করা হয়, তখন ডিফল্ট হিসেবে সকল ইউজার `/home` ডিরেক্টরির মধ্যে তৈরি হয়। তবে, আমরা চাইলে লোকেশন পরিবর্তন করতে পারি।

- `grep rafat /etc/passwd`এই কম্যান্ড রান করলে আমরা দেখতে পাবো যে, `rafat` ইউজারের হোম ডিরেক্টরি হচ্ছে `/home/rafat`

ইউজারের হোম ডিরেক্টরি পরিবর্তন করার জন্য, প্রথমে অন্য স্থানে একটি ডিরেক্টরি তৈরি করতে হবে। যেটা ইউজারের নতুন হোম ডিরেক্টরি হিসেবে কাজ করবে।

- `mkdir -p /newhome/rafat2` `/newhome/rafat2` একটি হোম ডিরেক্টরি তৈরি করা হয়েছে।
- `usermod -d /newhome/rafat2 rafat`

নোটঃ উপরের কমান্ডে `rafat` ইউজারের হোম ডিরেক্টরি পরিবর্তন করে `/newhome/rafat2` লোকেশনে দেওয়া হয়েছে।

- `grep rafat /etc/passwd` এখানে দেখতে পাবো `rafat` ইউজারের হোম ডিরেক্টরি পরিবর্তন হয়ে গেছে।

## Change User's Shell

এখন `rafat` ইউজারের শেল পরিবর্তন করবো। যখন কোনো ইউজার তৈরি করা হয়, তখন ডিফল্ট শেল হিসেবে `/bin/bash` থাকে তবে, ইউজার চাইলে তার ইচ্ছামত শেল পরিবর্তন করতে পারে। লিনাক্স সিস্টেমে বিভিন্ন ধরনের শেল থাকে `sh, bash, cshell, zshell, tcshell`।

এক এক ধরনের শেল আলাদা আলদা কমান্ড, কমান্ড সিনট্যাক্স, সাপোর্ট করে। তবে, অধিকাংশ ডিস্ট্রিবিউশন ব্যবহার করে `/bin/bash` কোনো ইউজার তার শেল ছাড়া সিস্টেমে লগইন করতে পারবে না, সেটা লোকাল কনসোল দিয়ে হউক অথবা রিমোট কনসোল।

মূলত যারা সিস্টেম অ্যাডমিন তাদের শেল হিশেবে `/bin/bash` `enable` থাকে আর যারা রেগুলার সার্ভিস ব্যবহারকারী ইউজার যেমনঃ `Mail, FTP, File` কিন্ত, সিস্টেম অ্যাডমিন না, তাদের ক্ষেত্রে `/sbin/nologin` থাকা উচিত। আর `/sbin/nologin` থাকলে কোনো ইউজার কোনো ভাবেই সিস্টেমে লগইন করতে পারবে না।

- `cat /etc/shells` লিনাক্স সিস্টেমে যত গুলো `shell` ইন্সটল আছে সেই লিস্ট দেখা যাবে।

```
/bin/sh
/bin/bash
/usr/bin/sh
/usr/bin/bash
```

- `grep rafat /etc/passwd` `rafat` ইউজারের বর্তমান `shell` দেখার জন্য।

এখানে আমরা দেখতে পাচ্ছি যে `rafat` ইউজারের `shell` হিসেবে `/bin/bash` সেট করা আছে, যার অর্থ `rafat` ইউজার সিস্টেমে লগইন করতে পারবে।

এখন `rafat` ইউজারের `shell` পরিবর্তন করে `/sbin/nologin` করবো।

- `usermod -s /sbin/nologin rafat` `rafat` ইউজারের `shell` পরিবর্তন করে `/sbin/nologin` করা হয়েছে।
- `grep rafat /etc/passwd` আউপুটে দেখা যাচ্ছে `rafat` ইউজারের `shell` পরিবর্তন হয়ে `/sbin/nologin` হয়েছে।
- `su rafat` unable to switch rafat user
- `usermod -s /bin/bash rafat` পুনরায় `rafat` ইউজারের লগইন `shell` `/bin/bash` সেট করা হয়েছে।
- `grep rafat /etc/passwd`
- `su rafat` আমরা চাইলে আবার চেক করে নিতে পারি।

## User's Acccount Lock and unlock / suspend & enable

- `grep student /etc/shadow` `student` ইউজারের পাসওয়ার্ড সম্পর্কিত তথ্য জানার জন্য। এখানে দেখতে পাচ্ছি `student` ইউজারের পাসওয়ার্ড দেওয়া আছে এবং শুরুতে `$6` মানে এ্যাকটিভ আছে।
- `usermod -L student` `student` ইউজারের একাউন্ট লক করে দেওয়া হয়েছে। এখানে `-L` মানে `Lock`
- `grep student /etc/shadow` এখানে দেখতে পাচ্ছি `student` ইউজারের পাসওয়ার্ড দেওয়া আছে এবং শুরুতে `!$6` মানে অ্যাকাউন্টি `Lock` আছে।

নোটঃ ইউজার লক করে দেয়া মানে হলো ইউজার কোথাও লগইন করতে পারবে না। যেমনঃ `FTP, Emil, web, system, server etc` কোথাও লগইন করতে পারবে না। কিন্তু `/sbin/nologin` করলে শুধুমাত্র সিস্টেমে লগইন করতে পারবে না।

- 1. `:!!:` কোনো পাসওয়ার্ড সেট করা হয়নি।
- 2. `:$6$sfs8vX/U:` পাসওয়ার্ড সেট করা আছে এবং একটিভ (Unlocked)।
- 3. `:!$6$sfs8vX/U:` পাসওয়ার্ড সেট করা আছে এবং ইন-একটিভ (Locked)।
- 4. `: :` পাসওয়ার্ড রিমুভ করে দেওয়া হয়েছে। ইউজার পাসওয়ার্ড ছাড়াই লগইন করতে পারবে।

Check: `Alt + Ctrl +  F3` (use username password)  
`Alt + Ctl + F3` প্রেস করে নতুন কনসোল দিয়ে লগইন করে দেখা যেতে পারে।

- `usermod -U student` `student` ইউজারের একাউন্ট পুনরায় Unlock করা হয়েছে। এখানে `-U` অর্থ `Unlock`।
- `grep student /etc/shadow`
- `id shahed` `shahed` নামে কোনো ইউজার আছে কিনা চেক করা হয়েছে।
- `userdel -r shahed`
- `id shahed` ইউজারের সকল তথ্য `home/mail/data/log` সবকিছু সহ মুছতে চাইলে `userdel -r` কমান্ড ব্যবহার করতে হবে।
- `cat /etc/passwd` সিস্টমে ইউজারের প্রোফাইল ডেটাবেজ চেক করলে `rafat` ইউজারের আর কোন তথ্য পাওয়া যাবে না।
- `groupdel support` সিস্টেম থেকে গ্ররুপ মুছে দেবার জন্য `groupdel` কমান্ড ব্যবহার করা হয়।
- `tail /etc/group` `/etc/group` ফাইল থেকে গ্রুপ রিমুভ হয়েছে কিনা সেটা দেখার জন্য।

## Linux SUDO Command

`root` ইউজার বা Administrative ইউজার দিয়ে লগইন করে কাজ না করা উচিত। সব সময় `sudo` ইউজার দিয়ে লগইন করে কাজ করা উচিত।

Working with Linux SUDO Privilege

এখন লিনাক্স সিস্টেমে `sudo` প্রিভেলেজ নিয়ে কাজ করা হবে, এইজন্য একজন নতুন ইউজার দরকার, সেজন্য `shakila` নামে একজন ইউজার তৈরি করে সেই ইউজারের পাসওয়ার্ড সেট করবো।

- `useradd shakila`
- `passwd shakila`
- `su - shakila`
- `useradd rashed`

`which useradd` রান করি তাহলে দেখতে পাবো যে এই কমান্ডটির পাথে `sbin` আছে। যেই কম্যান্ডগুলো `sbin` ডিরেক্টরি থেকে রান হয় সেগুলো রান করার জন্য `sudo` প্রিভিলিজ লাগে।

আমরা যদি `shakila` ইউজার দিয়ে লগইন করে `rashed` নামে ইউজার তৈরি করতে চাই, তাহলে আউটপুট `Permission denied` দিবে। কারণ, `shakila` ইউজার একজন রেগুলার ইউজার, সুতরাং `shakila` ইউজার অন্য কোনো ইউজারকে তৈরি করতে পারবে না।

তবে, যদি কোনো রেগুলার ইউজারকে সুপার ইউজারের প্রিভেলেজ দেওয়া হয়, তখন উক্ত রেগুলার ইউজার সুপার ইউজারের পার্মিশনের উপরে ভিত্তি করে বিভিন্ন আডমিনিস্ট্রেটিভ কমান্ড রান করতে পারবে।

## What is SUDO?

`SUDO` প্রিভিলেজের মাধ্যমে একজন রেগুলার ইউজারকে `student, munir, shakila` সুপার ইউজারের প্রিভেলেজ বা এক্সেস দেওয়া হয়। এক্ষেত্রে উক্ত রেগুলার ইউজারকে নির্দিষ্ট কোনো কমান্ড `single command`, বা একের অধিক কমান্ড `Group of commands`, বা চাইলে সিস্টেমের সকল কমান্ডের `All Commands` প্রভিলেজ (হুবুহু `root` ইউজারের মত) এক্সেস দেওয়া যেতে পারে।

লিনাক্স সিস্টেমে রুট `root` ইউজার কর্তৃক রেগুলার ইউজার তৈরি করা হয়। যেমনঃ `student, munir, hasan` এবং রেগুলার ইউজার যে সকল কমান্ড গুলা রান করতে বা চালাতে পারে সেইগুলা `/bin` ডিরেক্টরিতে থাকে। কিছু কমান্ডের লিস্ট নিচে দেওয়া হয়েছে।

- `rm, cp, mv`
- `mkdir, touch`
- `pwd, free -m`
- `ping, df -HT`
- `ip addr, tail`

লিনাক্স সিস্টেমে অ্যাডমিন ইউজার হচ্ছে রুট `root` ইউজার। `root` ইউজার সিস্টেমের সকল কমান্ড রান করতে বা চালাতে পারে যেইগুলা `/bin` এবং `/sbin` ডিরেক্টরিতে থাকে। `root` ইউজার কর্তৃক রান করা যায়, এমন কিছু কমান্ডের লিস্ট নিচে দেওয়া হয়েছে।

- `useradd, userdel, groupadd`
- `reboot, fdisk -l`
- `shutdown, poweroff, init`
- `setenforce`
- `which useradd` কোন কমান্ড কোন ডিরেক্টরি থেকে রান করে সেটা জানার জন্য `which` কমান্ড।
- `which pwd`
- `which touch`

## Editing sudo configuration File

এখন যদি কোনো রেগুলার ইউজার `shakila` কে সুপার ইউজারের `root` মত প্রিভিলেজ দিতে চাই, তাহলে `/etc/sudoers` ফাইলটিতে সেই ইউজারের নাম উল্লেখ করে প্রিভেলেজ দেওয়া যাবে। `/etc/sudoers` সরাসরি ওপেন করার জন্য নিচের কমান্ড `visudo` ব্যবহার করতে হবে।

## Permit shakila user for all commands

`visudo`

`:set nu`

- `100  root    ALL=(ALL)    ALL` এটার অর্থ হচ্ছে `root` ইউজার সকল কমান্ড `ALL` রান করতে পারে।
- `101  shakila  ALL=(ALL)    ALL` শধুমাত্র `shakila` ইউজারকে রুট `root` ইউজারের মত সম্পূর্ণ প্রভিলেজ `ALL` দেওয়া হয়েছে।

## Switch to shakila User

- `su - shakila`

এখন যদি আমরা `shakila` ইউজার দিয়ে আবার লগইন করে ইউজার তৈরি করার চেস্টা করি তাহলে একই `permission denied` মেসেজ দিবে।

- `useradd rashed`
- `sudo useradd rashed` SUDO ইউজার হিসেবে সিস্টেম থেকে Authentication নিতে চাইলে, শুরুতে `sudo` ব্যবহার করতে হবে। এবং কমান্ড চালানোর সময় `sudo` ইউজারের পাসওয়ার্ড দিতে হবে।

- `tail /etc/passwd`
- `exit`

## Permit trainer group for Group of commands

লিনাক্স সিস্টেমের কোনো গ্রুপের `/etc/group` সকল সদস্যদের SUDO প্রিভেলেজ দিতে চাইলে, গ্রুপের শুরুতে `%` দিয়ে উল্লেখ করতে হবে। তাহলে, গ্রুপের সকল ইউজারের ক্ষেত্রে SUDO প্রিভেলেজ পেয়ে যাবে।

`visudo `

```
108 %wheel 	ALL=(ALL)	ALL
109 %trainer ALL=(ALL)       ALL
```

## Permit only some specific commands

`visudo`

- `Cmnd_Alias NOC = /usr/sbin/useradd, /usr/sbin/usermod, /usr/sbin/groupadd`
- `102  shakila  ALL=(ALL) NOC`

যদি এমন হয় যে সব কম্যান্ড রান করতে পারবে তবে নির্দিষ্ট কিছু ছাড়া তাহলে `ALL, !NOC` অ্যালিয়েস (Alias) এর আগে `!` ব্যাবহার করতে হবে।

প্রথমে যে কমান্ড গুলা নির্দিষ্ট কোনো ইউজারের জন্য প্রভিলেজ দিতে চাই, সেই কমান্ড গুলা `/etc/sudoers` ফাইলের উপরের দিকে (Line 49) `Cmnd_Alias` অপশন দিয়ে উল্লেখ করতে হবে। তারপরে যে ইউজার কে উক্ত প্রিভেলেজ দেওয়া হবে, সেই ইউজারকে নিচের দিকে (Line 101) উল্লেখ করতে হবে।

## Password Policies

Working with `/etc/shadow` file

- `useradd lucky`
- `passwd lucky`
- `chage -d 0 lucky` ইউজার `lucky` তার পরবর্তী লগইনের সময় পাসওয়ার্ড পরিবর্তন করে নিতে পারবে। এখানে `-d` অর্থ `day` এবং `0` দিন।

Testing: `Alt + Ctl + F3` ব্যবহার করে চেক করা যাবে।

`grep lucky /etc/shadow`

```
lucky: $6 $ciiMIfom  $cPpqBIf2NOwan2byi.G6D0iM/g.tw7fcUyLDWIs.nbp0 :19570: 0 : 99999 : 7 :   :   :
1st        | 2nd                                              |     3rd    | 4th   | 5th     |  6th | 7th  | 8th  | 9th
```

- 1st - ইউজারের নাম।
- 2nd - ইউজারের পাসওয়ার্ড (:!!:, :!$:, :$6..:, : :)
- 3rd - সর্বশেষ কবে তার পাসওয়ার্ড পরিবর্তন করেছে। এখানে যে সংখ্যাটা দেখা যাচ্ছে সেটা দিন (19570) হিসেবে। আর এটা ধরা হয়, ০১-০১-১৯৭0 সাল থেকে আজ পর্যন্ত।
- 4th - পাসওয়ার্ডের মেয়াদ (Minimum)। এখানে ডিফল্ট ভাবে `0` থাকে, এর অর্থ হচ্ছে ইউজার চাইলে, যে কোনো সময় তার পাসওয়ার্ড পরিবর্তন করতে পারবে।
- 5th - পাসওয়ার্ডের মেয়াদ (Maximum)। ইউজার তার বর্তমান পাসওয়ার্ড `redhat` কত দিন ব্যবহার করতে পারবে।
- 6th - পাসওয়ার্ডের মেয়াদ উত্তীর্ণ হওয়ার কত দিন আগে নোটফিকেশন দিবে। (ওয়ার্নিং প্রিয়ড)।
- 7th - পাসওয়ার্ডের মেয়াদ উত্তীর্ণ হওয়ার পর কত দিন পর্যন্ত একাউন্ট একটিভ থাকবে।
- 8th - একাউন্টের মেয়াদ উত্তীর্ণ হওয়ার তারিখ।
- 9th - Reserved for future use.

- `useradd sathi`
- `chage -l sathi`
- `chage -l sathi` পাসওয়ার্ড সম্পর্কিত সকল তথ্য জানতে `chage -l` কমান্ড ব্যবহার করা যাবে।

```
Last password change                                    : MM DD, YYYY
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

```
- chage sathi
Minimum Password age [0]: 3
Maximum Password age [99999]: 30
Last Password Changed (YYYY-MM-DD): Press Enter (today)
Password Expiration Warning [7]: 5
Password Inactive [-1]: 5
Account Expiration Date (YYYY-MM-DD) [-1]: YYYY-MM-DD
```

Note: If press Enter account never expire

- `chage -l sathi`
- `tail /etc/shadow | grep sathi`
- `date`
- `date` MMDDHHMMYY (MM = Month, DD = Day, HH= Hour, MM = Minute, YY=Year)
- `date`
- `vim /etc/login.defs` এই ফাইল থেকে ভ্যালু পরিবর্তন করে ইউজারের ডিফল্ট পাসওয়ার্ড সেটিংস পরিবর্তন করা যাবে।

- 131 PASS_MAX_DAYS 60
- 132 PASS_MIN_DAYS 0
- 133 PASS_MIN_LEN 6
- 134 PASS_WARN_AGE 5

- `useradd rakib`
- `passwd rakib`

## Login regular user

- `Alt + Ctrl + F3` নতুন কনসোল ওপেন করে লগইন করে চেক করা হবে পাসওয়ার্ড দেওয়ার অপশন আসে কিনা।
- `Alt + Ctrl + F2` পুনরায় গ্রাফিক্যাল ইউজার ইন্টারফেসে ফিরে যাওয়ার জন্য `Alt + ctrl + F2` প্রেস করতে হবে।

## Homework

- 1. create a group "rhcsa19"
- 2. user "oly, liza, rana" member of 'rhcsa19' group
- 3. all user's password set 'redhat'
- 4. lock liza user's account
- 5. Lock rafat user's shell
- 6. Create user robin using UID '5001'
- 7. Allow user 'oly' as a sudo user
- 8. User 'rana' must be reset his password on next login

`ifconfig | grep ether | head -1 | cut -d " " -f 10`