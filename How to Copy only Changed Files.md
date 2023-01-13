---
title: # Sample Post
menu_order: 1
post_status: publish
post_excerpt:
taxonomy:
    category:
        - Shell
    post_tag:
        - rsync
        - bash
        - shell
        - backup
comment_status: open
---

# How to Copy only Changed Files

I was wondering today, as my mind refuses to remember command line options, what is the right command to copy data from /data-extern/Images to /data-grave/Images that is not yet in the target destination or that has different file size or timestamp in the destination? I want to maintain all timestamps. Operating system is linux, rsync is available, I just can't remember the right rsync commands, and a previous copy using cp -av /data-extern/Images /data-grave was interrupted somewhere in the middle.

Answer is that to do that, you use the following command:

```bash
rsync -avz --ignore-existing --no-perms --no-owner --no-group /data-extern/Images/ /data-grave/Images/
```

The options used in this command are:

-   `-a` for archive mode, which preserves file permissions, ownership, timestamps, etc.
-   `-v` for verbose output
-   `-z` to compress file data during the transfer. This is optional: If you are copying on your local machine, it can slow down the process significantly if you copy files that anyway can't be compressed.
-   `--ignore-existing` to skip files that already exist in the destination
-   `--no-perms`, `--no-owner`, `--no-group` to prevent rsync from trying to change permissions, ownership, or group on the destination files, preserving the timestamps.

You can also use this command if the previous copy was interrupted, it will only copy the missing files.
