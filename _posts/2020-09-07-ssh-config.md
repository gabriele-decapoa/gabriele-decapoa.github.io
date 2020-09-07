---
layout: post
title: SSH Config
categories:
  - Best_practices
  - Cloud_development
---

Sometimes you need to use different SSH keys to connect to different remote hosts.
You could be a little bit lazy and you don't want to remember which is the right SSH key to use.  
The best way is to configure your SSH agent in order to use the right SSH key based on the host, using a configuration file named `.ssh/config`.

This is a simple textfile with a well-known syntax.  
For example, if you would like to connect to host `bar.example.com` using SSH key `id_rsa_bar` and user `bar`, and also connect to host `foo.example.com` using SSH key `id_rsa_foo` and user `foo`, the file will be like the following.

```
Host bar.example.com
 User bar
 IdentityFile ~/.ssh/id_rsa_bar
Host foo.example.com
 User foo
 IdentityFile ~/.ssh/id_rsa_foo
 ```