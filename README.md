SSH keys are the bee's knees
---
---
Imagine having a static authentication method amongst all your devices. How about passwordless logins?
 
This is possible with SSH keys. When you generate a key, you get two pieces of information; your public and private keys. These are aptly named, as your public key can be shared around without the risk of compromising your systems, but the private key must be kept safe.

---
 
The method for using a key is relatively simple. In a vanilla setup where both machines are linux, we assume than openssh is installed.
All of our magic will happen in the .ssh/ directory of your home folder.
 
Generating keys in it's simplest form is easy:

```
ssh-keygen
```
 
This will ask you where you wish to save it, and if you'd like to put a password on it. If no password is provided, no password will be needed once one has been setup.

This will generate two files:

```
.ssh/
├── id_rsa
└── id_rsa.pub

```

While your ~/.ssh/ may have more files, these are the ones we're concentrating on these right now. Don't lose focus.

You can post your id_rsa.pub anywhere, safely give it to most anyone. Tweet it, I don't care.

Your id_rsa, however, needs to stay safe. While I recommend placing a password upon it during generation (providing an extra security layer if someone gets ahold of your private key). Passwordless can be fine, if you know your host machine is safe.

The contents of your public key look something like this:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD0HfOJrC3mFDZl3D2DB60DOHFSH9pwwlWe5nS5vKgzk+N9MDnGVdyIHSUBoMC0byYKJ2afjyhuL5rwto7VE4eNCeTQ8lIS4Xik5JBT3iycNR3NYNKmH7HE9MYjazgDkZpaDw1XMjA0W1D1jRwK255XwiMRt2MMQUHoILkSpGfqtpT0nsbNRqbgUmzhbiBgxRCmN8hImjDL/kezc1K2PLEnuzI7tmZ3YO3l38N6JVWzNmOBdh4IuZD6pXNynC1CgXPO+tt3xSjr8vDCbgxvA+1AjfslpHmBJFhf3FEAxXsY/1MlpAk2fbFVxQ3SsqlM/71LPVDNI8jbVwC+D1teaTiD me@localhost
```

This is in turn put on the remote machine.

Let's login to the remote machine to install our key now.

```
ssh root@example.com
Warning: Permanently added 'example.com' (RSA) to the list of known hosts.
root@example.com's password:
root@example:~# cd ~/.ssh/
root@example:~# echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD0HfOJrC3mFDZl3D2DB60DOHFSH9pwwlWe5nS5vKgzk+N9MDnGVdyIHSUBoMC0byYKJ2afjyhuL5rwto7VE4eNCeTQ8lIS4Xik5JBT3iycNR3NYNKmH7HE9MYjazgDkZpaDw1XMjA0W1D1jRwK255XwiMRt2MMQUHoILkSpGfqtpT0nsbNRqbgUmzhbiBgxRCmN8hImjDL/kezc1K2PLEnuzI7tmZ3YO3l38N6JVWzNmOBdh4IuZD6pXNynC1CgXPO+tt3xSjr8vDCbgxvA+1AjfslpHmBJFhf3FEAxXsY/1MlpAk2fbFVxQ3SsqlM/71LPVDNI8jbVwC+D1teaTiD me@localhost" >> authorized_keys
root@example:~# logout
Connection to root@example.com closed.

```
Now, we see that logging in requires no password, or the password that we chose upon generation.

```
ssh root@example.com

root@example:~#
```

There are multiple types of security available with ssh-keygen, the most common are RSA and DSA (specified at generation with -t). I would recommend gonig with RSA, as generation is slower, but authentication is faster.
As of openSSH 5.7, a more secure type known as ecdsa is available, but not widely used as putty and gnome-keyring do not currently support this type of encryption.

You may also specify key length with the -b argument. The default length being 2048 bits.

In widows, we'll assume you're using PuTTY, or a PuTTY derivative.

Open up "PuTTYgen.exe" to generate our public and private keys.
