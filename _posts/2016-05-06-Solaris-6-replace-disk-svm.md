---
layout: post
type: post
tags:
- svm
- Solaris 6
- Unix
published: true
title: Solaris 6 Disk replacement
---

SOLARIS 6 DISK REPLACEMENT

In this example, the disk to be replaced is c0t0d0, which is currently mirrored with c0t8d0

Run the below commands.

 cd /usr/opt/SUNWmd/sbin # This is where all the commands for disk replacement are located.


Disk to be replaced c0t0d0 --->c0t8d0 to be used for labelling(since they're mirrored)
Run command:
~~~ bash
cfgadm -av c0
~~~

For more information about the cfgadm command and it's uses please visit: #http://docs.oracle.com/cd/E23824_01/html/821-1462/cfgadm-1m.html

Now run metastat -p to determine the mirrors and submirror
Note, save the above information somewehere, this will be used in detaching and also reattaching the mirrors

~~~ bash
bash-3.2$ metastat -p
d0 -m d10 d20 1
d10 1 1 c0t0d0s1
d20 1 1 c0t8d0s1
d3 -m d13 d23 1
d13 1 1 c0t0d0s0
d23 1 1 c0t8d0s0
d4-m d14 d24 1
d14 1 1 c0t0d0s5
d24 1 1 c0t8d0s5
d5 -m d15 d25 1
d15 1 1 c0t0d0s3
d25 1 1 c0t8d0s3
d6 -m d16 d26 1
d16 1 1 c0t0d0s1
d26 1 1 c0t8d0s1


#Now proceed to detach the mirrors

metadetach -f d0 d10
metadetach -f d3 d13
metadetach -f d4 d14
metadetach -f d5 d15
metadetach -f d6 d16


Now we need to clear the metadevices that we detached from above.


metaclear d10 d13 d14 d15 d16 && ./metastat -p

delete the state database replicas using the below command

./metadb -d c0t0d0s7

Show the new config.
./metastat -p

Now insert the New Disk and after it's initialized continue below

format
Label it now? y
format> label
ready to label disk, continue? y
format>q

##clear slice "7"

metadb -d c0t0d0s7

#Copy the partition table from c0t8d0
prtvtoc /dev/rdsk/c0t8d0s2 | fmthard -s - /dev/rdsk/c0t0d0s2
metadb -a -c 3 c0t0d0s7


#Configure the metadevices on the new disk

metainit d10
metainit d13
metainit d14
metainit d15
metainit d16


#Reattach the mirrors and the submirrors

metattach d0 d10
metattach d3 d13
metattach d4 d14
metattach d5 d15
metattach d6 d16

#Run below command to check the status of the disk re-sync status

while true; do clear; ./metastat |grep sync ; sleep 15; done
~~~
