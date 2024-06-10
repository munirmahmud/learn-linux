# Day 09: File / Directory permission & owernship

Working with Linux File / Directory permission & owernship:

### Our Working Directory will be /linux/day9/

- `mkdir /linux`
- `cd /linux`
- `ls`
- `mkdir day9`
- `cd day9`
- `touch file1` বর্তমান ডিরেক্টরি `day9` এর মধ্যে `file1` নামে একটা ফাইল তৈরি করা হল।
- `mkdir dir1` বর্তমান ডিরেক্টরি `day9` এর মধ্যে `dir1` নামে একটা ডিরেক্টরি তৈরি করা হল।
- `cp /etc/passwd` `/etc/passwd` ফাইলটি কপি করে বর্তমান ডিরেক্টরি `day9` মধ্যে নিয়ে আসা হয়েছে।
- `ll`

```
d rwxr-xr-x.  2  root  root     6  May 10 17:54  dir1
- rw-r--r--.  1  root  root  2662  May 10 17:55  passwd
- rw-r--r--.  1  root  root     0  May 10 17:54  file1

1      2      3   4     5       6          7       8

1: file/dir types (লিনাক্সে মোট সাত ধরনের (-, d, b, c, l, s, p ) ফাইল/ডিরেক্টরি পাওয়া যাবে)
2: file/dir permission (User + Group + Others + ACL)
3: file/dir link (Hard Link) - ফাইলের ক্ষেত্রে '1' এবং ডিরেক্টরির ক্ষেত্রে '2' থাকবে, তবে ভিতরে যত ডিরেক্টরি থাকবে তার উপরে ভিত্তি করে সংখ্যা বাড়বে।
4: file/dir owner (যে ইউজার ফাইল/ডিরেক্টরিটি তৈরি করেছে তার নাম থাকবে)
5: file/dir group owner - (ইউজার যে গ্রুপের সদস্য সেই গ্রুপের নাম থাকবে)
6: file/dir size (byte) - ফাইল হলে '0' বাইট, এবং ডিরেক্টরি হলে '6' বাইট।
7: file/dir modify date
8: file/dir name
```

## Working with link file

লিঙ্ক ফাইলের অর্থ, একটি ফাইলের সাথে আরেকটি ফাইলের লিঙ্ক থাকে। অর্থাৎ কোনো ফাইলে কোনো লেখা/টেক্সট মোডিফাই/আপডেট করলে অপর ফাইলটিও একই ভাবে আপডেট হবে। যেমনঃ উইন্ডোজ অপারেটিং সিস্টেমের ক্ষেত্রে শর্টকাট ফাইল। ফাইলের শর্টকাট গুলা থাকে আমাদের ডেক্সটপে এবং সেগুলার সোর্স (Origin) থাকে অন্য লোকেশনে। আমরা যদি ডেক্সটপ থেকে সেই সকল শর্টকাট ফাইলের উপরে ক্লিক করি, তাহলে মূল সোর্স (Origin) থেকে রান করবে।

লিনাক্স সিস্টেমে দুই ধরণের লিঙ্ক ফাইল আছেঃ

## Type of link file

- `Hard link` ফাইল গুলার আইনোড নাম্বার `inode` একই হবে। অর্থাৎ দুইটি ফাইলের মাঝে যদি হার্ডলিঙ্ক করা হয়, তাদের কনটেন্ট, সাইজ, পার্মিশন একই হবে, শুধু নাম আলাদা হবে। এক্ষেত্রে মূল ফাইলটি ডিলিট হলেও লিংক ফাইলের কোনো পরিবর্তন বা সমস্যা হবে না। হার্ডলিঙ্ক শুধুমাত্র ফাইলের ক্ষেত্রেই করা যাবে।

- `Soft link` ফাইল গুলার আইনোড নাম্বার `inode` ভিন্ন হবে। দুইটা ফাইল/ডিরেক্টরির মধ্যে যদি সফট লিঙ্ক করা হয়, তাদের কনটেন্ট একই হবে শুধু নাম আলাদা হবে। সফটলিঙ্ক বা `symbolik` লিঙ্ক ফাইলের শুরুতে `l` থাকে। এক্ষেত্রে যদি মূল ফাইলটি ডিলিট করা হয়, তাহলে লিংক ফাইল কোনো কাজ করবে না। সফট লিঙ্ক ফাইল এবং ডিরেক্টরি উভয়ের ক্ষেত্রেই করা যায়।

```
ls -li
tail passwd
ln -s  passwd passwd-soft - softlink তৈরির ক্ষেত্রে '-s' ব্যবহার করতে হবে।
ln passwd passwd-hard - hardlink তৈরির কমান্ড।
ll
tail passwd-hard
tail passwd-soft
ls -li

echo "The End" >> passwd
tail -3 passwd
tail -3 passwd-hard
tail -3 passwd-soft
rm -f passwd
ll
tail -3 passwd-hard
tail passwd-soft
ln -s /home/student/Documents studentdoc
```

নোটঃ একই অ্যাপ্লিকেশন একাধিক লোকেশনে হস্ট করতে গেলে লিঙ্ক করার প্রয়োজন পড়ে।

## Permission Field no: 2, 4 & 5

`ll`

```
 - rw-r--r--. 1  root  root    0   Sep 26 09:52  file1
 d rwxr-xr-x. 2  root  root  4096  Sep 26 09:33  dir1
      (2)        (4)   (5)

  - rw-  r--  r--  .  = file1  - ডিফল্ট ভাবে লিনাক্সের ফাইলের কোনো এক্সিকিউট (x) পার্মিশন থাকে না।
  d rwx  r-x  r-x  .  = dir1   - ডিফল্ট ভাবে লিনাক্সের ডিরেক্টরিতে ইউজার, গ্রুপ এবং অন্যান্য ইউজারদের জন্য এক্সেকিউট (x) পার্মিশন থাকে।
    (u)  (g)  (o) (A)
```

- u = user
- g = group
- o = others
- A = ACL Permission (.)

- r = read (4) - ফাইলের ক্ষেত্রে 'read' অর্থ ফাইলটি পড়তে পারবে এবং ডিরেক্টরির ক্ষেত্রে read অর্থ, ডিরেক্টরি টা কপি করা যাবে।
  w = write (2) - ফাইলের ক্ষেত্রে 'write' অর্থ ফাইলটি এডিট এবং সেভ করা যাবে। ডিরেক্টরির ক্ষেত্রে write অর্থ, ডিরেক্টরির ভিতরে paste করা যাবে।

- x = execute (1) - ফাইলের ক্ষেত্রে 'execute' পার্মিশন দরকার নাই। কিন্ত, ফাইলটি যদি, প্রোগাম/স্ক্রিপ্ট/এপ্লিকেশন হয়, তাহলে অবশ্যই execute পার্মিশন থাকতে হবে। ডিরেক্টরির ক্ষেত্রে 'execute' হচ্ছে ডিরেক্টরির মধ্যে প্রবেশ করার জন্য, অর্থাৎ যে ডিরেক্টরিতে 'execute' পার্মিশন নাই, সেখানে প্রবেশ করা যাবে না।

- = no permission (0) - এক্ষেত্রে 'read/write/execute' কোনো পার্মিশন থাকছে না।

- . = ACL Permission (+) - ফাইল/ডিরেক্টরি উভয়ের ক্ষেত্রে এখানে dot (.) থাকবে। কিন্ত, যখন ACL (File Access Control) এপ্লাই করা হবে তখন, এখানে Dot (.) এর পরিবর্তে '+' সাইন চলে আসবে।

```
 - rw- r-- r-- .  = file1  (644)
 d rwx r-x r-x .  = dir1   (755)

   rw- = 4 + 2 + 0 = 6 (U)     rwx = 4 + 2 + 1 = 7 (U)
   r-- = 4 + 0 + 0 = 4 (G)     r-x = 4 + 0 + 1 = 5 (G)
   r-- = 4 + 0 + 0 = 4 (O)     r-x = 4 + 0 + 1 = 5 (O)
   -------------------        --------------------
 		  644			  = 755
```

নোটঃ ডিফল্ট ফাইল পার্মিশন = 644 এবং ডিরেক্টরি পার্মিশন = 755 তবে, আমরা আমাদের প্রয়োজনে পার্মিশন লেভেল পরিবর্তন করতে পারি।

- `Maximum File Permission: 666       =>  (rw- rw- rw-)`
- `Maximum Directory Permission: 777  =>  (rwx rwx rwx)`

## Test

- 740 = rwx r-- ---
- 735 = rwx -wx r-x
- 642 = rw- r-- -w-
- 503 = r-x --- -wx
- 150 = --x r-x ---
- 326 = -wx -w- rw-
- 467 = r-- rw- rwx
- 631 = rw- -wx --x

## Linux Standard File Permission

এখানে `file1` নিয়ে কাজ করা হবে। যেখানে একটা গ্রুপ থাকবে `students` নামে এবং `students` গ্রুপের মেম্বার হবে `sadat, sumon, malek`। সেই সাথে একজন গেস্ট ইউজার `Others` থাকবে `tamim` নামে।

Group: Members others
====== ======= ======
students: malek, sumon, sadat all (except group members)

- `groupadd students`
- `useradd -G students malek`
- `useradd -G students sadat`
- `useradd -G students sumon`
- `useradd tamim`
- `grep students /etc/group`

এখন `file1` কে `Malek` ইউজারের `Ownership` এবং `students` গ্রুপের `Group Ownership` দেওয়া হবে। `malek` ইউজারের জন্য পূর্ণ `rwx` পার্মিশন দেওয়া হবে। পাশাপাশি `students` গ্রুপের ইউজারদের শুধু `read (r)` পার্মিশন থাকবে এবং অন্য ইউজারের/গেস্ট অর্থাৎ `others` এর ক্ষেত্রে কোনো প্রকার পার্মিশন থাকবে না।

```
user: (malek) : full permission (rwx)
group: students: read (r--)
others: others : no   (---)
```

`ls -l`

- `chown malek file1` `chown` কমান্ড ব্যবহার করে `Ownership` পরিবর্তন করা যায়।
- `ls -l`

- `chgrp students file1` `chgrp` কমান্ড ব্যবহার করে `Group Ownership` পরিবর্তন করা যায়।
- `ls -l`

- `chmod 740 file1` `chmod` কমান্ড ব্যবহার করে পার্মিশন `rwx` পরিবর্তন করা যায়।
- `ls -l`

### Ownership Testing

```
su malek
ls
echo "This is malek" > file1 - malek can rw
cat file1
exit
```

### Groupowner Testing

```
su sumon
ls
cat file1
echo "This is sumon" > file1  -  Permission Denied (only read)
exit
```

### Other User Testing

```
su tamim
cat file1
echo "This is tamim" > file1
exit
```

## Linux File Access Control List (ACL)

- যদি একটা ফাইল/ডিরেক্টরিতে একাধিক `Ownership` বা `Group Ownership` দরকার হয়।
- একটা ফাইল/ডিরেক্টরিতে আলাদা আলদা ইউজার এবং গ্রুপের আলাদা আলাদা পার্মিশন দরকার হলে।
- নির্দিষ্ট কোনো গ্রুপ থেকে নির্দিষ্ট কোনো ইউজারের প্রিভিলেজ তুলে নিতে চাইলে বা পরিবর্তন করতে চাইলে।
- আমরা যদি গেস্ট `others` ইউজার থেকে কাউকে স্পেশাল প্রিভেলেজ দিতে চাই।

আমরা যদি উপরে উল্লেখিত ব্যাপারে চিন্তা করি, তাহলে আমাদের স্ট্যান্ডার্ড ফাইল/ডিরেক্টরি পার্মিশন ব্যবহার করে সেটা করা সম্ভব না। সেক্ষেত্রে ব্যবহার করতে হবে `ACL` পার্মিশন।

`ACL` এর পূর্ণরুপ হচ্ছে `Access Control List`, যেটা আমরা ফাইল এবং ডিরেক্টরি উভয়ের ক্ষেত্রে ব্যবহার করতে পারি। `ACL` সিকিউরিটি সার্ভিস লিনাক্স ফাইল সিস্টেমের `ext4/xfs` উপরে নির্ভর করে। যেহেতু আমাদের ফাইল সিস্টেম `xfs` সুতরাং এটা ডিফল্ট ভাবে এনাবেল থাকে।

## Working Directory

- `ls`
- `touch tutorial profile`
- `ll`

এই ল্যাব ডেমো প্র্যাকটিস করার জন্য `jack, rose, tomy` নামে তিন জন ইউজার এবং `support` নামে গ্রুপ একাউন্ট তৈরি করা হয়েছে।

- `useradd jack`
- `useradd rose`
- `useradd tomy`
- `groupadd support`

## ACL Test

`getfacl profile` ফাইল/ডিরেক্টরিতে `ACL` পার্মিশন দেখার কমান্ড।

## ACL Command Options

- `m = modify`
- `x = remove`
- `b = blank `
- `R = Recrusivly`

## User ACL

Lab: আমরা নিচের উদাহরণে, `profile` ফাইলে `jack` এর জন্য `rwx`, ইউজার `rose` এর জন্য `r--` এবং ইউজার `tomy` এর ক্ষেত্রে কোনো পার্মিশন দেওয়া হবে না।

- `setfacl -m u:jack:rwx,rose:r--,tomy:---  profile` ফাইল/ডিরেক্টরিতে `ACL` পার্মিশন দেওয়ার কমান্ড।
- `ll`
- `getfacl profile`

উপরের কমান্ড আউটপুটে আমরা দেখতে পাচ্ছি, ইউজার `jack` এর ক্ষেত্রে `rwx` অ্যাড হয়েছে। পাশাপাশি ইউজার `rose` এর ক্ষেত্রে `r--` এবং `other` ইউজার `tomy` এর ক্ষেত্রে পার্মিশন তুলে নেওয়া হয়েছে।

- `su jack`
- `cat profile`
- `echo this is Jack > profile`
- `cat profile`
- `exit`

- `su rose`
- `cat profile`
- `echo this is Rose >> profile`
- `exit`

- `su tomy`
- `cat profile`
- `echo this is Tomy >> profile`
- `exit`

## Group ACL

- `setfacl -m g:support:rw-  tutorial `
- `getfacl tutorial`

## Directory Permission

- `mkdir acldir`
- `touch acldir/file1`
- `touch acldir/file2`
- `ls -l acldir`
- `setfacl -R -m u:rose:rwx acldir` (-R for recursively)
- `ls -l acldir`
- `getfacl acldir`

## ACL Remove (user)

- `setfacl -x u:rose: profile`
- `getfacl profile`

## ACL Remove (Gruop)

- `setfacl -x g:support: tutorial`
- `getfacl tutorial `

## Remove ACL from File:

- `setfacl -b tutorial `

## Linux SUID, SGID and Sticky Bit Concept

লিনাক্সের স্ট্যান্ডার্ড পার্মিশনের পাশাপাশি কিছু স্পেশাল পার্মিশন ব্যবহার করা হয়।

- স্ট্যান্ডার্ড পার্মিশন প্রকাশ করা হয় `r=read, w=write, x=execute, -= no permission`
- স্পেশাল পার্মিশন প্রকাশ করা হয় `S, S, T / s, s, t` দিয়ে।

- Read=4 (r)
- Write=2 (w)
- Execute=1 (x)
- no permission = 0 (-)

- `rwx = 4+2+1 =  7`

উপরের `Standard Permission` এর সাথে যোগ হবে লিনাক্স স্পেশাল `SUID/SGID/Sticky` পার্মিশন।

- 4 = SUID (s,S) - SUID পার্মিশন ব্যবহার হয় Owner ফিল্ডে (1st field) এবং এটা প্রকাশ করা হয় 'S' অথবা 's' দিয়ে।

- 2 = SGID (s,S) - SGID পার্মিশন ব্যবহার হয় Group ফিল্ডে (2nd field) এবং এটাও প্রকাশ করা হয় 'S' অথবা 's' দিয়ে।

- 1 = Sticky bit (t, T) - Sticky পার্মিশন ব্যবহার হয় Other ফিল্ডে (3rd field) এবং এটা প্রকাশ করা হয় 'T' অথবা 't' দিয়ে।

নোটঃ যদি ফাইল/ডিরেক্টরিতে আগে থেকেই এক্সিকিউট পার্মিশন থাকে, তাহলে স্পেশাল পার্মিশন সেট করার পরে ছোট হাতের `s, s, t` থাকবে, আর যদি কোনো এক্সিকিউট পার্মিশন (x) না থাকে সেক্ষেত্রে `S, S, T`

## Example

- `rwx r-x r-x = rws r-s r-t  - 'x'` বিট সহ স্পেশাল পার্মিশনের ব্যবহার করা হয়।
- `rw- r-- r-- = rwS r-S r-T  - 'x'` বিট বাদে স্পেশাল পার্মিশনের ব্যবহার করা হয়।

`u   g   o`

```
x755 (where x = Special Permission for directory)
x644 (where x = Special Permission for file)
```

একটা ডিরেক্টরির ক্ষেত্রে যদি চিন্তা করি,

```
4 + 755 = SUID       (1st Field)
2 + 755 = SGID       (2nd Field)
1 + 755 = Sticky bit (3rd Field)
```

## Working with Test SUID

`SUID` যখন কোনো ফাইল/কমান্ডকে রেগুলার ইউজার কে সুপার ইউজারের `root` প্রিভেলেজে রান করার দরকার হয়, তখন সেই ফাইল বা কমান্ডের উপরে `SUID` সেট করা হয়। তার অর্থ উক্ত ফাইল/কমান্ডটি সুপার ইউজারের `root` (file Owner) পাশাপাশি যে কোনো রেগুলার ইউজার একই প্রিভিলেজে এক্সেস/রান করতে পারবে।

- `which passwd`
- `ls -l /bin/passwd`

- `ls -l /usr/bin/ls`

- `su student`
- `ls /root`

- `exit`
- `ls -l /usr/bin/ls`

- `chmod 4755 /usr/bin/ls` SUID Permission

- `ls -l /usr/bin/ls`
- `su student`
- `ls /root`
- `exit`

`Class work Login as 'student' user and view log file '/var/log/messages' using 'tail'`

```
Hints: [root@node1 day9]# tail -5 /var/log/messages
Hints: [student@node1 day9]$ tail -5 /var/log/messages
```

## Working with SGID

`SGID` যখন ব্যবহার করা হবে, তখন গ্রুপের মেম্বারদের মধ্যে কোনো ইউজার যদি কোনো ফাইল/ডিরেক্টরি তৈরি করলে, তার প্রিভিলেজ আটোমেটিক্যালি গ্রুপের অন্যান্য মেম্বাররা পেয়ে যাবে। অর্থাৎ প্রতিটি ফাইলের গ্রুপ মেম্বারশিপ `Inherit` হবে।

- `mkdir resource`
- `ls -ld resource`

- `grep students /etc/group`

- `chmod 770 resource`
- `chgrp students resource`

- `ls -ld resource`

- `su malek`
- `touch resource/malek1`
- `exit`

- `su sadat`
- `touch resource/sadat1`
- `exit`
- `ll resource`

- `chmod 2770 resource` SGID Permission
- `ls -ld resource`

- `su sumon`
- `touch resource/sumon1`
- `ll resource`

## Working with Sticky Bit

লিনাক্সে ডিরেক্টরির ম্যাক্সিমাম পার্মিশন হচ্ছে `777` এবং এই ম্যাক্সিমাম পার্মিশন দেওয়ার পরে যে কোনো ইউজার চাইলে এই ডিরেক্টরির মধ্যকার ফাইল ডিলিট করে দিতে পারে।

কিন্ত, আমরা যদি উক্ত ডিরেক্টরির উপরে `Sticky bit` পার্মিশন এ্যাপ্লাই করি তাহলে, যে কোনো ইউজার ডিরেক্টরির মধ্যে ফাইল তৈরি করতে বা পেস্ট করতে পারবে কিন্ত ডিলিট করতে পারবে না।

- `ls -ld /tmp`

- `mkdir mydir1`
- `mkdir mydir2`

- `ll`
- `chmod 777 mydir1` standard permission
- `chmod 1777 mydir2` sticky bit

- `ls -ld mydir1 mydir2`

- `touch mydir1/file1`
- `touch mydir2/file2`

- `su student`
- `rm mydir1/file1` এটার প্যারেন্ট ডিরেক্টরিতে ফুল পার্মিশন এটা ডিলিট হয়ে যাবে।
- `rm mydir2/file2` এটার প্যারেন্ট ডিরেক্টরিতে যেহেতু `sticky` বিট সেট করা আছে সুতরাং এটা ডিলিট হবে না।

## Home Work: 02

- Copy the file '/etc/passwd' to /opt

  - The file '/etc/passwd' is owned by the 'root' user.
  - The file '/etc/passwd' belongs to the group 'root'
  - The file '/etc/passwd' should not be executable by anyone.
  - The user 'robin' is able to read and write '/opt/passwd'
  - The user 'rafat' can neither write nor read '/opt/passwd'
  - All other users (current or future) have the ability to read '/opt/passwd

- Create a collaborate directory "/shared/sysusers"
  - Group ownership of "/shared/sysusers" is 'sysusers'
  - Permission should be readeable, writeable and access by 'sysusers' group but not to any other user.
  - Files created in "/shared/sysusers" automatically have group ownership set to the 'sysusers' Group.
