---

title: How to Install a Foreign Language for LibreOffice MacOS
menu_order: 1
post_status: publish
post_excerpt: Getting French language support for LibreOffice 7.1 to work under MacOS posed some challenges.
taxonomy:
    category:
        - Computer
        - LibreOffice
    post_tag:
        - Computer
        - LibreOffice
        - MacOS
        - Java

---

# Motivation

I tried to install an additional language pack for LibreOffice 7.1 under MacOS Ventura. I downloaded `LanguageTool-5.7.oxt` from [here](https://extensions.libreoffice.org) and started it up. After some wait time, this led to the error 

```
LibreOffice requires Oracle's Java Development Kit (JDK)
on macOS 10.10 or greater to perform this task.
Please install them and restart LibreOffice.
https://hub.libreoffice.org/InstallJava/?LOlocale=en-US
```

# Solution

Hence, I downloaded the latest JDK [from Oracle](https://download.oracle.com/java/19/latest/jdk-19_macos-aarch64_bin.dmg). I then restarted installation of the LanguageTool, which worked without issues.



---
Related: [[Computer/MacOS/- -|MacOS]]
