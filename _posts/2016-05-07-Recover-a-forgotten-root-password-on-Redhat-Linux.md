---
layout: post
type: post
tags: 
  - root password
  - Linux
  - Redhat
published: true
title: Recover a forgotten root password on Redhat Linux
---


Boot up/ restart your server.
As soon as the grub menu loads, hit **e** to edit.

Look for this line,  **rhgb quiet LANG-en_US.UTF-8**
Add **rd.break** at the end of that line.
Then hit **ctrl + x** to boot into single user mode


-**mount -o remount,rw /sysroot**

- **chroot /sysroot**

Now you can change the password by typing passwd root and creating new password

- **passwd root**--change the password

- **exit**

Finally, relable all files when system boots up, this is important so as to maintain due to SElinux contexts

- **touch / .autorelabel**#if this is not done passwd change will not be successful
~~~
