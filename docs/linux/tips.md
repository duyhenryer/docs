!!! note "Work in progress"
    This document is a work in progress.


```
ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime
sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
```