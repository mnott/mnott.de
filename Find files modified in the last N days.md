---
title: Find files modified in the last N days
menu_order: 1
post_status: publish
post_excerpt: This is a MacOS compatible version looking for files that have a modification date N days in the past.
taxonomy:
    category:
        - computer
        - shell
        - bash
    post_tag:
        - computer
        - shell
        - bash
comment_status: open
---

If you want to check for files modified in the last N days, you may come across [this solution](https://superuser.com/questions/294161/unix-linux-find-and-sort-by-date-modified/453734#453734):

```bash
find . -type f -ls | awk '{print $(NF-3), $(NF-2), $(NF-1), $NF}' | sort
```

Problem is, this cuts out filenames with spaces (because `$(NF-3)` etc. looks through the fields starting from the back of the line), you can start with the front, like so:

```bash
find . -mtime -30  -type f -ls | awk '{$1=$2=$3=$4=$5=$6=$7=""; print substr($0,8)}' | sort
```

This solution shows at the same time how to look for files modified in the last 30 days; it works because all fields before the date field are set to blank, and then what remains is the field `$0`, which is all of what remains of the line.

The downside of that solution is that it does not include the year. A much easier variant is this:

```bash
find . -mtime -30 -type f -print0 | xargs -0 ls -ltr
```

If you want to expand on this, and put it as a function into your `.zshrc`, you can do this:

```bash
mls() { if [[ $#>0 ]]; then d=$1; shift; find . -mtime -$d $* -type f -print0 | xargs -0 ls -ltr; else echo Usage: $0 days \[further find options\]; fi }
```

This allows you to have a command `mls` which will list, in the current directory and subdirectories, the files modified the last N days sorted by timestamp; and it also allows you to pass in additional options to find. So for example:

```bash
mls 10 -iname "*php"
```

This would traverse the directory and give you all `php` files changed during the last 10 days.


---
Related: [[🧠 Ideaverse/Other/- -|Other]]


<mark style="margin-top: 100; background-color: #3B3836; color: #494942">Created: 1`$=dv.span(dv.current().file.ctime)`</mark>