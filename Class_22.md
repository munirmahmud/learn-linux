# Class 22:

`yum whatprovides nmtui` কোন কমান্ড কোন সফটওয়্যার প্রভাইড করে থাকে তা দেখা যায়। Redhat 7 বার তার নিচের ভার্সনের জন্য `yum provides nmtui`

DNS এর তথ্য /etc/resolv.conf ফাইলে থাকে।

local name resolution এর জন্যা ইউজ করা হয় /etc/hosts ফাইল।

ওয়ার্ল্ডে ১৩ বা ১৬ টা রুট DNS আছে।

`yum remove software/packa_name -y` কোন সফটওয়্যার রিমুভ করতে চাইলে। যেমনঃ `yum remove httpd -y`

`rpm -qi software/packa_name` কোন সফটওয়্যারের ইনফরমেশন জানার জন্য। যেমনঃ `rpm -qi httpd`

`yum search software/packa_name` রিপজিটরিতে কোথায় কোথায় ঐ নামটি আছে তা দেখার জন্য। `yum search httpd`

`yum list` সব রিপজিটরি লিস্ট দেখার জন্য। এই লিস্ট গুলো মূলত `/etc/yum.repos.d` ডিরেকটরিতে যেই লিস্টগুলো আছে তার উপর ভিত্তি করে দেখায়।

`yum deplist httpd` ডিপেন্ডেন্সি লিস্ট দেখার জন্য। অর্থাৎ কোন সফটওয়্যার ইন্সটল করতে গেলে তার কোন ডিপেন্ডেন্সি আছে কিনা এবং থাকলে কি কি ডিপেন্ডেন্সি সফটওয়্যার আছে তার বিস্তারিত দেখায়।

`dnf downgrade software/packa_name` কোন সফটওয়্যার তার নিচের ভার্সনে যাওয়ার জন্য।

Varsion: 1.0.1 >> major.minor.hot-fix

Update মানে হল মেজর ভার্সনটিকে আপডেট করা আর
Upgrade মানে হল minor বা hotfix কে upgrade করা।

Update করলে পুরনো ভার্সনকে রিমুভ করে দিয়ে নতুন ভার্সন ইন্সটল করে।

`yum clean all` সব cache রিমুভ করার জন্য।

`yum repolist all` সকল রিপলিস্ট দেখার জন্য।

`yum repolist or yum repolist enabled` ইনেবলড রিপলিস্ট দেখার জন্য।

`yum repolist disabled` ডিজেবলড রিপলিস্ট দেখার জন্য।

`yum repolist epel` ডিজেবলড রিপলিস্ট দেখার জন্য।

টারমিনালে `yum` লিখে ট্যাব বাটনে ২ বার প্রেস করলে আরও কিছু কমান্ড লিস্ট পাওয়া যাবে।

`yum grouplist` iso ইন্সটল করার সময় বিভিন্ন অপসন্স বা ইন্সটলেশন মুড থেকে আমরা নির্দিষ্ট কোন একটা মুড সিলেক্ট করে ইন্সটল করি যেমনঃ `Server with GUI` অথবা `Server` অথবা `Minimal Install` অথবা `Custom Operating System` ইত্যাদি। iso ইন্সটল করার পরে যদি আমরা অন্য কোন মুডে সিস্টেম নিতে চাই বা ইন্সটল করতে চাই তাহলে এই কমান্ডের মাধ্যমে আমরা গ্রুপগুলোর নাম জানতে পারি।

`yum groupinstall "Server with GUI" -y` যেমন আমরা যদি ঐ গ্রুপ লিস্ট থেকে কোন একটা গ্রুপ ইন্সটল করতে চাই তাহলে এই কমান্ড।

`yum groupinfo "Server with GUI"` Server with GUI এর ম্যান্ডেটরি টুলস সম্পর্কে জানতে পারি।

`yum shell` yum এর শেল রান করার জন্য। yum এর শেলে কোন কমান্ড রান করার জন্য yum কিওয়ার্ড টি লিখা লাগবে না। যেমনঃ `install httpd`

যদি কোন রিপজিটরি ডিজেবল থাকে এবং সেই রিপজিটরি থেকে কোন সফটওয়্যার ইন্সটল করার প্রয়োজন পরে তাহলে তাহলে সেই রিপজিটরিকে ইনেবল করে তারপরে ইন্সটল করতে হয়। আমরা চাইলে এটা কমানডের মাধ্যমেও করতে পারি। অর্থাৎ কমান্ড রান করার সময় সে ঐ রিপজিটরিকে ইনেবল করে সফটওয়্যারটিকে ইন্সটল করে আবার ডিজেবল করে দিবে।

`yum --enablerepo highavailability install pcs -y` `--enablerepo` এর পরে Repository ID দিতে হবে।

`yum --disablerepo extras install httpd -y` extras রিপজিটরি ডিজেবল করে তারপরে সে httpd সফটওয়্যারটি ইন্সটল করবে।

## Systemd / System Daemon

Systemd হল একটা প্রসেস একে মাদার প্রসেসও বলে। সিস্টেম প্রপারলি রান হওয়ার জন্য কিছু প্রসেস অটমেটিক্যালি রান করে আবার আমরা যখন কোন সফটওয়্যার বা কমান্ড রান করি তখন ঐ সফটওয়্যার বা কমান্ড ঠিকভাবে কাজ করার জন্য এক বা একাধিক প্রসেস রান করে। এই সবগুলো প্রসেস systemd এর অধীনে রান করে।

`pstree` দিয়ে আমরা টারমিনালে প্রসেসগুলোকে Tree Structure এ দেখতে পারি।

`pstree | less` Process Tree কে line by line দেখার জন্য।

`initd` initialization systemd RHEL 6 এবং এর আগের ভার্সনগুলোতে ছিল। এক্ষেত্রে কম্যান্ড শুরু হবে `service` দিয়ে।

initd FIFO (First In First Out) style এ কাজ করে থাকে যার কারনে একটু স্লো পারফরম করে। initd synchronously কাজ করে যার কারনে সিস্টেম রান হতে অনেক সময় নেয়।

systemd প্যারালালি সব গুলো সফটওয়্যার (যেগুলো on boot এ start করা আছে) রান করে এতে করে সিস্টেম রান হতে খুব কম সময় নেয়।

initd তে একসাথে একাধিক সফটওয়্যার রান করা যায় না কিন্তু systemd তে রান করা যায়।

`initd` এর জন্য `service` আর `systemd` এর জন্য `systemctl` কমান্ড ব্যাবহৃত হয়।

`systemctl start httpd` এই কমান্ডের মাধ্যমে আমরা `httpd` সফটয়্যারটি রান করলাম। এইভাবে একসাথে একাধিক সফটওয়্যার রান করা যায় যেমনঃ `systemctl start httpd ftpd nginx` ইত্যাদি।

`systemctl -t help` unit type দেখার জন্য।

`-t for type`

Unit type এর মধ্যে একটা unit হল target এই target unit আবার ৭ রকমের। run level ৭ টাই হল এই target।

Target unit এর মধ্যে একাধিক unit কাজ করতে পারে। যেমনঃ runlevel3 একটা Target unit এর মধ্যে `service, mount socket, automount, device, swap` ইত্যাদি কাজ করে।

| Status                    | Description                                                                                                             |
| :------------------------ | :---------------------------------------------------------------------------------------------------------------------- |
| loaded                    | The unit's configuration file has been successfully read and processed.                                                 |
| Active (exited)           | Successfully executed the one-time configuration and after execution, the unit is neither running an active process     |
| nor waiting for an event. |
| Active (running)          | Successfully executed the one-time configuration and after execution, the unit is running one or more active processes. |
| Active (waliting)         | Successfully executed the one-time configuration and after execution, the unit is waiting for an event.                 |
| Inactive (dead)           | Either the one-time configuration failed to execute or not executed yet.                                                |

| Unit Type | Description                                                                                                                                      |
| :-------- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| Target    | A group of units that defines a synchronization point. The synchronization point is used at boot time to start the system in a particular state. |
| Service   | A unit of this type starts, stops, restarts or reloads a service daemon such as Apache webserver.                                                |
| Socket    | A unit of this type activates a service when the service receives incoming trafic on a listening socket.                                         |
| Device    | A unit of this type implements device-based activation such as a device driver.                                                                  |
| Mount     | A unit of this type controls the file-system mount point.                                                                                        |
| Automount | A unit of this type provides and controls on-demand mounting of file systems                                                                     |
| Swap      | A unit of this type encapsulates/activates/deactivates swap partition.                                                                           |
| Path      | A unit of this type monitors files/directories and activates/deactivates a service if the specified file or directory is accessed.               |
| Timer     | A unit of this type activates/deactivates specified service based on a timer or when the set time is elapsed.                                    |
| Snapshot  | A unit that creates and saves the current state of all running units. This state can be used to restore the system later.                        |
| Slice     | A group of units that manages system resources such as CPU, and memory.                                                                          |
| Scope     | A unit that organizes and manages foreign processes                                                                                              |
| busname   | A unit that controls DBus system                                                                                                                 |

<!-- 1:8 -->
