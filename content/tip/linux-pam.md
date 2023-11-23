+++
title = 'Linux : ssh action before and after session'
draft = false
categories = ['Linux', 'System', 'Ssh']
tags = ['Linux', 'PAM', 'ssh']
comments = false # Enable Disqus comments for specific page
authorbox = true # Enable authorbox for specific page
+++

# Execute script when SSH open and close session 

Edit file /etc/pam.d/sshd and add line like this :

```
session     optional    pam_exec.so quiet /usr/local/sbin/pam_ssh.sh
```

pam_ssh.sh script is like this : 

```
#!/bin/bash

if [ "$PAM_TYPE" = "open_session" ] && [ "$PAM_USER" == "admin" ] ; then

    <do something>

fi 

if [ "$PAM_TYPE" = "close_session" ] && [ "$PAM_USER" == "admin" ] ; then

    <do something>

fi

exit 0
```
