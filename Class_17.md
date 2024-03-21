# Class 17: ln, locate, find

File/Directory link (Hard Link) - ফাইলের ক্ষেত্রে '1' এবং ডিরেক্টরির ক্ষেত্রে '2' থাকবে, তবে ভিতরে যত ডিরেক্টরি থাকবে তার উপরে ভিত্তি করে সংখ্যা বাড়বে। ডিরেক্টরিতে '2' থাকার কারণ হলো তার নিজের লিঙ্ক এবং তার প্যারেন্ট ডিরেক্টরির লিঙ্ক।

লিঙ্ক ফাইলের অর্থ, একটি ফাইলের সাথে আরেকটি ফাইলের লিঙ্ক থাকে। অর্থাৎ কোনো ফাইলে কোনো লেখা/টেক্সট মোডিফাই/আপডেট করলে অপর ফাইলটিও একই ভাবে আপডেট হবে। যেমনঃ উইন্ডোজ অপারেটিং সিস্টেমের ক্ষেত্রে শর্টকাট ফাইল। ফাইলের শর্টকাট গুলা থাকে আমাদের ডেক্সটপে এবং সেগুলার সোর্স (Origin) থাকে অন্য লোকেশনে। আমরা যদি ডেক্সটপ থেকে সেই সকল শর্টকাট ফাইলের উপরে ক্লিক করি, তাহলে মূল সোর্স (Origin) থেকে রান করবে।

লিনাক্স সিস্টেমে দুই ধরণের লিঙ্ক ফাইল আছেঃ

1.  Hard link - ফাইল গুলার আইনোড নাম্বার (inode number) একই হবে। অর্থাৎ দুইটি ফাইলের মাঝে যদি Hardlink করা হয়, তাদের content, size, permission একই হবে, শুধু নাম অথবা ফাইলের লোকেশন আলাদা হবে। এক্ষেত্রে মূল ফাইলটি ডিলিট হলেও লিংক ফাইলের কোনো পরিবর্তন বা সমস্যা হবে না। Hardlink শুধুমাত্র ফাইলের ক্ষেত্রেই করা যাবে।

N.B: `ZFS file system`

2.  Soft link - ফাইল গুলার (inode number) ভিন্ন হবে। দুইটা ফাইল/ডিরেক্টরির মধ্যে যদি সফট লিঙ্ক করা হয়, তাদের content একই হবে শুধু নাম অথবা ফাইলের লোকেশন আলাদা হবে। Soft link বা Symbolik ফাইলের শুরুতে ইংরেজি লেটার 'l' থাকে। এক্ষেত্রে যদি মূল ফাইলটি ডিলিট করা হয়, তাহলে লিংক ফাইল কোনো কাজ করবে না। সফট লিঙ্ক, ফাইল এবং ডিরেক্টরি উভয়ের ক্ষেত্রেই করা যায়।

Command: `ln -s main_file linked_file` softlink তৈরির ক্ষেত্রে `-s` ব্যবহার করতে হবে।

Command: `ln main_file linked_file` Hardlink তৈরি করার command

`touch symlink.txt`

`ln -s symlink.txt symlink-soft.txt`

`ll`

`echo "Hello symlink" >> symlink.txt`

`cat symlink.txt` এবং

`cat symlink২.txt` করে আমরা দেখতে পাবো যে উভয় ফাইলে একই content আছে।

এখন `rm -f symlink.txt` command রান করলে দেখবো, আমাদের symlink-soft.txt ফাইলটি আর কাজ করছে না।

এখন আমরা hardlink তৈরি করা দেখবো।

`touch hardlink.txt`

`ln hardlink.txt hardlink2.txt`

`ll`

`echo "Hello hardlink" >> hardlink.txt`

`cat hardlink.txt` এবং

`cat hardlink২.txt` করে আমরা দেখতে পাবো যে উভয় ফাইলে একই content আছে।

এখন `rm -f hardlink.txt` command রান করলে দেখবো, আমাদের hardlink2.txt ফাইলটি রান করতে কোন সমস্যা হচ্ছে না।

## locate command

locate কমান্ডের মাধ্যমে লিনাক্স সিস্টেমের মধ্যে যে কোনো file/directory খুঁজে বের করা যাবে। locate command প্র্যাকটিস করার জন্য 'root' ইউজারের প্রিভিলেজ লাগবে।

আমরা এই ল্যাবটি প্রাকটিস করার জন্য /opt/ directory তে কিছু ফাইল তৈরি করবো।
`touch /opt/testing{01..50}.py`

`locate /opt/*.py` এই কমান্ডটি কাজ না করলে লোকাল ডেটাবেজ আপডেট দিতে হবে। তার জন্য এই কমান্ডটি রান করতে হবে `updatedb`।

শুধুমাত্র directory search করার ক্ষেত্রে `/directory_name/` ২ টা forward slash এর মধ্যে directory নাম বলে দিতে হবে অন্যথায় অতিরিক্ত search result দেখাবে। যেমন আমি 'opt' নামক directory search করবো তাহলে কমান্ডটি হবে `locate /opt/`

যদি case sensitive ignore করতে চাই তাহলে `locate -i file_or_directory_name`

Search করে কতোগুলো রেজাল্ট পেল তা দেখার জন্য
`locate -i /opt/file_or_directory_name -c`

Total search result থেকে নির্দিষ্ট সংখক রেজাল্ট দেখার জন্য `locate -i /opt/file_or_directory_name -n 2`। আমরা চাইলে একসাথেও লিখতে পারি যেমনঃ `-n2` এবং এটি কমান্ডের পরেও লিখতে পারি। যদি বর্তমান ডিরেক্টরিতে কোন ফাইল খুজতে চাই তাহলে ফাইলের শুরুতে forward slash অর্থাৎ `/` দিতে হবে।

## find command

`find / -name mail` mail নামে যে কোনো ফাইল/ফোল্ডার রুট `/` ফাইল সিস্টেমে খোঁজার জন্য।

1. find হলো আমাদের কমান্ড

2. `/` (forward slash) হলো ফাইল পাথ। অর্থাৎ আমরা যেই লোকেশনে ফাইলটি খুজবো সেই লোকেশনটি বলে দিতে হবে। এ ক্ষেত্রে পুরো সিস্টেমে ঐ ফাইলটি খুজবে।

3. `-name` এর পরে ফাইল অথবা ফোল্ডারের নাম বলে দিতে হবে। `-name` অপশনটি ছাডাও কাজ করবে কিন্তু শেষে একটা ওয়ার্নিং মেসেজ দিবে এবং কিছু অতিরিক্ত search result দেখাবে। exact match এর জন্য আমরা `-name` অপশনটি ব্যাবহার করবো।

4. `mail` হচ্ছে আমাদের search keywork অর্থাৎ আমরা যা খুজতে চাই।

উপরের সার্চ কমান্ডটি Case sensitive। যদি case insensitive ভাবে সার্চ করতে চাই তাহলে `-name` এর সাথে `i` যোগ করে দিলেই হবে অর্থাৎ `-iname`

`find /var -name mail` নির্দিষ্ট লোকেশনে `/var` ডিরেক্টরির মধ্যে `mail` নামে কিছু খোঁজার জন্য।

`find / -type d -name ssh` `ssh` নামে ডিরেক্টরি খুঁজে বের করার জন্য।

`find / -type f -name ssh` `ssh` নামে ফাইল খুঁজে বের করার জন্য।

`find / -type b` সিস্টেমের সকল ডিভাইস ফাইল খুঁজে বের করার কমান্ড।

`find / -size +100M` `100 MB` এর উপরের সাইজের সকল ফাইল খুঁজে বের করার জন্য।

`find / -type f -inum 156587168` এই কমান্ড দিয়ে hard link ফাইল গুলো বের করতে পারি। প্রথমে ফাইলের inode number বের করে নিতে হবে।

আমরা ফাইল পাথ অথবা পুরো সিস্টেমে এ খুঁজার জন্য `/` এর পরে `-type d` অপশন দিয়ে ফাইল টাইপ বলে দিতে পারি। এক্ষেত্রে এই টাইপগুলো থেকে যেকোনো একটি ইউজ করতে পারি। `b c d f l p s`

`b stands for block device`

`c stands for character`

`d stands for directory`

`f stands for file (regular file)`

`l stands for link file`

`p stands for pipe line file`

`s stands for socket layer file`

উপরে আমরা `-name -iname -type -inum` এই options গুলো শিখেছি।

এ ছাডাও আরও options আছে। যেমনঃ `-user -group -perm -exec -atime -mtime -ctime -amin -mmin -cmin -uid -gid -maxdepth -size` ইত্যাদি।
