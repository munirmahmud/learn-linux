# Class 18:

আমরা find কমান্ড ইউজ করে ইউজার ওয়াইজ সার্চ করতে পারি।

`find / -type f -iname ".*" -user munir`

আমরা find কমান্ড ইউজ করে গ্রুপ ওয়াইজ সার্চ করতে পারি।

`find / -type f -iname ".*" -grup devops`

যে সকল ফাইল কোন নির্দিষ্ট ইউজার এবং ঐ ফাইল কোন নির্দিষ্ট গ্রুপের অধিনে তৈরি করা সেগুলো খুজে বের করার জন্য।

`find / -type f -iname ".*" -user munir -grup devops`

ইউজার আইডি দিয়েও আমরা সার্চ করতে পারি।

`find / -type f -iname ".*" -uid 1000`

গ্রুপ আইডি দিয়েও সার্চ করা যায়।

`find / -type f -iname ".*" -gid 1000`

পারমিশন দিয়ে সার্চ করার জন্য।

যে সকল ফাইলের ইউজার পারমিশন 7 অর্থাৎ `read, write, & execute` আছে এবং অন্য কোন পারমিশন নাই সেগুলো সার্চ করবে।

`find / -type f -iname "*" -perm 700` (Exact Search)

`find / -type f -iname "*" -perm 2700` (Exact Search) GUID Bit Permission দিয়ে সার্চ করার জন্য।

`find / -type f -iname "*" -perm 4700`

`-700` দিয়ে বুঝানো হচ্ছে যে ইউজারের জন্য 7 পারমিশন থাকতেই হবে কিন্তু গ্রুপ এবং আদারসের জন্য যে কোন পারমিশন থাকেতে পারে।

`find / -type f -iname "*" -perm -700` (Minimum requirement 7)

যেকোনো একটা ম্যাচ করলেই সার্চ রেজাল্টে নিয়ে আসবে। এটা রেড হ্যাট ৭ এ যেকোন একটা ম্যাচ করলে হত কিন্তু ভার্সন ৯ এ `-perm -700` এর মতো কাজ করে।

`find / -type f -iname "*" -perm /700` (hardly used)

স্পেশাল পারমিশন দিয়েও সার্চ করতে পারি। যেমন গ্রুপের স্পেশাল পারমিশন `2` দিয়ে সার্চ করা যায়। তার মানে হল গ্রুপে স্পেশাল পারমিশন থাকতেই হবে কিন্তু ইউজার এবং আদারসের ক্ষেত্রে যে কোন পারমিশন থাকতে পারে। এভাবে ইউজার এবং আদারসের খেত্রেও আমরা `4 & 1` স্পেসাল পারমিশন ইউজ করে সার্চ করতে পারি।

`find / -type f -iname "*" -perm -2000` গ্রুপের স্পেশাল পারমিশন দিয়ে সার্চ।

`find / -type f -iname "*" -perm -4000` ইউজারের স্পেশাল পারমিশন দিয়ে সার্চ।

`find / -type f -iname "*" -perm -1000` আদারসের স্পেশাল পারমিশন দিয়ে সার্চ।

`find / -type f -iname "*" -perm u+rwx` or `find / -type f -iname "*" -perm 700`

`find / -type f -iname "*" -perm -4700` or `find / -type f -iname "*" -perm -u+ srwx`

`find / -type f -iname "*" -perm -u+srwx,g+r,o+x`

### Size

`find / -size -6M` `5.0 - 6MB` এর মধ্যকার সকল সাইজের সকল ফাইল খুঁজে বের করার জন্য।