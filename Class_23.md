# RHCSA Class 23: Systemd, Systemctl Runlevel

Target হল একটা একটা রান লেভেল।

By default, unit related ফাইলগুলো `/usr/lib/systemd/system` এই ডিরেক্টরিতে থাকে। আমরা যখন `systemctl list-units --type service` কমান্ড রান করি তখন টারমিনালে `service unit` এর কিছু আউটপুট আসে সেগুলোই এই ডিরেক্টরি থেকে নিয়ে দেখায়।

`cd /usr/lib/systemd/system` এই লোকেশনে `ls | grep httpd` কমান্ড রান করলে সিস্টেমে যদি `httpd` থাকে তাহলে আউটপুট আসবে।

`systemctl status httpd` এই কমান্ডের মাধ্যমে কোন এক বা একাধিক `service` এর status জানা যায়। এই কমান্ড যখন রান করি তখন `systemctl` কমান্ড `systemd` এর কাছে রিকুয়েস্ট করে status এর জানার জন্য। systemd তার Tree এর মধ্যে খুঁজে দেখবে আছে কিনা, যদি থাকে তাহলে কোন অবস্থায় আছে।

`pstree` কমান্ডের মাধ্যমে আমরা সকল প্রসেস দেখতে পারি tree structure এ। systemd প্যারেন্ট প্রসেস এবং বাকিগুলো চাইল্ড প্রসেস হিসেবে আছে।

`systemctl start httpd` কোন সারভিস active করার জন্য `systemctl start` এবং তারপর সার্ভিস নাম দিতে হয়। এতে করে সার্ভিসটি স্টার্ট হয়।

`systemctl enable httpd` কোন সার্ভিস অন বুটে স্টার্ট করার জন্য। অর্থাৎঃ সিস্টেম যখন স্টার্ট হয় তখন যাতে সার্ভিসটি স্টার্ট হয়।

আমরা চাইলে একাধিক সার্ভিস নামও একসাথে কমান্ডের সাথে বলে দিতে পারি। যেমনঃ `systemctl enable httpd vsftpd mysqld` ইত্যাদি।

`systemctl enable --now httpd` আলাদা আলাদা `start` এবং `enable` কমান্ড না দিয়ে একসাথে `start` এবং `enable` করা যায়। এখেত্রেও একাধিক সারভিসের নাম বলে দেয়া যায়।

`systemctl disable --now httpd vsftpd` আলাদাভাবে `disable` এবং `stop` কমান্ড না দিয়ে একসাথে `disable` এবং `stop` করা যায়। এক্ষেত্রেও একাধিক সার্ভিসের নাম বলে দেয়া যায়।

`systemctl is-active httpd` এভাবেও কোন সার্ভিস `active/inactive` আছে তা জানা যায়। এখানেও একাধিক সার্ভিস নাম বলে দেয়া যায়।

`systemctl is-enabled nginx` এভাবেও কোন সার্ভিস `enabled/disabled` আছে তা জানা যায়। এখানেও একাধিক সার্ভিস নাম বলে দেয়া যায়।

`systemctl restart httpd` সার্ভিসে কোন রকম চেঞ্জ করার পরে ঐ সার্ভিসকে রিস্টার্ট দিতে হয়।

`systemctl stop httpd` কোন সার্ভিস রান থাকলে তা স্টপ করার জন্য। এখানে `httpd` সার্ভিসকে স্টপ করলাম।

`systemctl disable httpd` কোন সার্ভিস `enabled` থাকলে তা `disabled` করার জন্য। এখানে `httpd` সার্ভিসকে ডিজেবল্ড করলাম।

### Runlevels / Targets

`systemd` কর্তৃক সকল সার্ভিস লোড হওয়ার পরে সিস্টেম কোন মোডে (CLI, GUI, Rescue) রান করবে এটা নির্ভর করে `Systemd target` সেটিংসের উপর।

systemd =>

- 1. GUI (graphical.target)
- 2. CMD (muti-user.target)
- 3. Rescue.target
- 4. Emergency.target

নোটঃ `system maintenance, troubleshooting, or recovery` করার উদ্দেশ্যে `rescue.target` সেট করা হয়। এই মোডে শুধু মাত্র `root` (Single User) ইউজারের শেল প্রিভেলেজ থাকে এবং লিমিটেড কিছু সার্ভিস লোড হয়।

`init 3` কমান্ড রান করলে আমরা `run level 3` তে যেতে পারবো। `runlevel` কমান্ড রান করে চেক করতে পারি। এই যে একটা মোড থেকে আরেকটা মোডে সুইচ করার যে টেকনিক তা `systemctl` কমান্ড দিয়ে করা যায়।

`systemctl isolate runlevel3` এই কমান্ডের মাধ্যমে আমরা `runlevel 3` তে যেতে পারি।

উপরে আমরা `runlevel` চেঞ্জ করেছি কিন্তু এটা পারমানান্ট না। সিস্টেম রিবুট হলে `runlevel` তার আগের অবস্থানে ফিরে আসবে।

`systemctl get-default` সিস্টেমের বর্তমান মোড সম্পর্কে জানা যায়।

`systemctl set-default multi-user.target` or `systemctl set-default runlevel3` এই কমান্ডের যেকোনটি দিয়ে আমরা নির্দিষ্ট রান লেভেল বা মোড সেট করতে পারি। `reboot` দেয়ার পরে রান লেভেল বা মোড পরিলক্ষিত হবে।

`systemctl reboot / init 6 / reboot` সিস্টেম রিবুট দেয়ার জন্য।

`systemctl shutdown/poweroff / init 0 / shutdown/poweroff` সিস্টেম শাটডাউন দেয়ার জন্য।

`/etc/systemd/system/` এই ডিরেক্টরিতে `default.target` নামে একটা ফাইল আছে যেটার একটা লিঙ্ক ফাইল এবং এর মেইন ফাইলের লোকেশন হলো `/usr/lib/systemd/system/graphical.target` কমান্ড দিয়ে যেই চেঞ্জেসগুলো হয় তার কনফিগারেশন ফাইল এই লোকেশনে হয়।
