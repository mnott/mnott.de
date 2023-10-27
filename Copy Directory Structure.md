---
title: Copy an empty Directory Structure
menu_order: 1
post_status: publish
post_excerpt: This is a MacOS compatible version using Python to copy a directory structure.
taxonomy:
    category:
        - Computer
        - Shell
        - Bash
        - Python
        - MacOS
    post_tag:
        - Computer
        - Shell
        - Bash
        - Python
        - MacOS
comment_status: open
---

If you want to copy an empty directory stucture, from the current directory to some target directory, you can do this:

```python
python -c 'import os,sys;dirs=[ r for r,s,f in os.walk(".") if r != "."];[os.makedirs(os.path.join(sys.argv[1],i)) for i in dirs if not os.path.exists(os.path.join(sys.argv[1],i))]' ~/some/target
```

It does not overwrite directories or files in the target directory.

You just slap the target directory to the end of it.


---
Related: [[🧠 Ideaverse/MacOS/- -|MacOS]]


<mark style="margin-top: 100; background-color: #3B3836; color: #494942">Created: 1`$=dv.span(dv.current().file.ctime)`</mark>