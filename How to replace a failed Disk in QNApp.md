---
title: How to replace a failed Disk in QNApp
menu_order: 1
post_status: publish
post_excerpt: This shows how to replace a failed disk in QNApp.
taxonomy:
    category:
        - Computer
        - Shell
        - Bash
    post_tag:
        - Computer
        - Shell
        - Bash
comment_status: open
---

# Overview

We again had a failed disk on our QNApp. Here is what we did.

# Identify the Disk

Logging onto, in our case, [https://192.168.0.253](https://192.168.0.253), we were able to verify that we had a failed disk:

![qnapp_failed_disk_01.png](qnapp_failed_disk_01.png)



![qnapp_failed_disk_02.png](qnapp_failed_disk_02.png)

# Replace the Disk

We basically just shut down the device and then swapped the faulty disk and restarted the device.

# Scanning for Bad Blocks

After switching the device back on, the disk was showing bad blocks:

![qnapp_failed_disk_03.png](qnapp_failed_disk_03.png)


so we started scanning for bad blocks:

![qnapp_failed_disk_04.png](qnapp_failed_disk_04.png)



![qnapp_failed_disk_05.png](qnapp_failed_disk_05.png)


This process took several hours.

# Adding the Disk to the RAID

After the disk check, the RAID showed being degraded, and the disk not being a member of the RAID. The screen shot below was taken after solving that problem; before, it showed not "Rebuilding", but "Degraded", for the RAID Group 1, and "Not Member" instead of "Rebuilding" for NAS Host: Disk 4:

## Identifying that the new Disk was not member of the RAID

![qnapp_failed_disk_06.png](qnapp_failed_disk_06.png)

## Activating Telnet

For some reason, we weren't able to enable SSH, so we chose to activate telnet:

![qnapp_failed_disk_07.png](qnapp_failed_disk_07.png)


![qnapp_failed_disk_08.png](qnapp_failed_disk_08.png)


## Log on to the RAID using Telnet

With that setting, we could connect to the device:

![qnapp_failed_disk_09.png](qnapp_failed_disk_09.png)

It showed us a nice menu, so we chose `q` to exit out of it:

![qnapp_failed_disk_10.png](qnapp_failed_disk_10.png)

After that, we confirmed that we really wanted to get out of that:

![qnapp_failed_disk_11.png](qnapp_failed_disk_11.png)

This then dropped us to a prompt. We went back to the root directory, doing `cd /`:

![qnapp_failed_disk_12.png](qnapp_failed_disk_12.png)

## Confirm the missing Disk

From the shell, we first ran

```bash
md_checker
```

![qnapp_failed_disk_13.png](qnapp_failed_disk_13.png)

This showed that the disk 4 (`/dev/sdc3`) was still marked as missing. We next used 

```bash
cat /proc/mdstat
```

to identify the volume:

![qnapp_failed_disk_14.png](qnapp_failed_disk_14.png)

This allowed us to run `mdadm` to confirm the removed status:

```bash
mdadm --misc --detail /dev/md1
```

![qnapp_failed_disk_15.png](qnapp_failed_disk_15.png)


## Add the missing Disk

Using

```bash
mdadm /dev/md1 --add /dev/sdc3
```

we were able to add the disk, after which calling again

```bash
mdmadm --misc --detail /dev/md1
```

confirmed that the disk had been added and the rebuild had been started automatically:

![qnapp_failed_disk_16.png](qnapp_failed_disk_16.png)


## Verify the Rebuilding is in Process

Using again

```bash
md_checker
```

we could confirm the rebuild being in process:

![qnapp_failed_disk_17.png](qnapp_failed_disk_17.png)

This was also visible in the front end:

![qnapp_failed_disk_18.png](qnapp_failed_disk_18.png)

And as soon as it was above 1%, we could see the process in the backend, using again `mdadm`:

![qnapp_failed_disk_19.png](qnapp_failed_disk_19.png)

## Exit from the Backend

Just using `exit`, we left the backend and now just had to wait until the process completed.

