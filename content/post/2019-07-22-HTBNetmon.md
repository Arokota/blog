---
title: HTB - Netmon
subtitle:
date: 2019-07-22
type: "post"
---

Well the summer has officially kicked off and I find myself getting a bit rusty, so I thought I'd hop onto HTB and play around some and make sure I earned that piece of paper now that says I'm a certified professional.  (Maybe I'll write a blog post on that journey in the future)

I VPNed into HTB and I decided to start off with a machine that was focused on more of a CVE style compromise vs CTF style.  I've been looking into exploit development and bug hunting at work lately and I thought that this would help scratch that itch.  

I booted up Nmap and gave the machine a nice throroughly scan.
![Nmap Scan](/img/htbnmap.png)
Interesting! My attention, as I'm sure yours is, was immediately drawn to the PRTG service that seems to be running on port 80.  I've dealt with PRTG in a few past network admin jobs, it's a fantastic tool for any network admin to be able to keep track of the devices running in his/her network and receive alerts based on their status.
![PRTG Login Page](/img/htbprtg.png)
Well, as I figured the login interface would be pretty logged down.  I did throw the default credentals `prtgadmin:prtgadmin` at it for fun. No luck :P

Next up was connecting to that juicy FTP server.  Plz anonymous access?
### User Access
![FTP Access](/img/htbftp.png)
YES! My next step was to and immediately see if with whatever low priviledges I had if I could read the user flag.
![Getting user](/img/htbuser.png)
To my luck they had `user.txt` sitting in the `C:\Users\Public` directory and I was able to download it locally and read it with no problem.
![User Hash](/img/htbuserhash.png)
### System Access
At this point I spent a good amount of time digging around in the configuration files for PRTG.  Once I navigated down to `C:\ProgramData\Paessler\PRTG Network Monitor` I found a few files named `PRTG Configuration.dat` `PRTG Configuration.old` and `PRTG Configuration.old.bak`.  
![PRTG Config](/img/htbprtgconfig.png)
I quickly downloaded all three and begin to look through them.  Not too long after I'm looking through the one with the `.bak` extention I discover these beautufiul plaintext credentials :) 
![PRTG Creds](/img/htbprtgcreds.png)

I spent an unfortunate amount of time sitting here now trying to figure out why I could not log in.After banging my head against a while for a little bit and resting the box I finally decided to change the date from `PrTg@dmin2018` to `PrTg@dmin2019` and it worked.
![Logging in](/img/htbloggingin.png)

From this point on I was able to google the version of PRTG and found [this](https://www.codewatch.org/blog/?p=453) disclosure writeup that provided a lot of insight into a Command Injection vulnerability.

In PRTG you are able to choose what your notifications will do when they are triggered, one of these things being executing a program.  Paessler has this logged down and doesn't let you just run any old program, however they do provide some base `.bat` and `.ps1` scripts that you can run and don't sanitize input.

I quickly ran a test with the command **test.txt; echo "test" >> C:\Testing123.txt**
![Command Injection Test](/img/htbcommandinjtest.png)
BOOM! Hell yeah! All I had to do next was change the command to read 
**test.txt; type C:\Users\Administrator\Desktop\root.txt > C:\hash.txt**
![Root Hash](/img/htbroothash.png)

Overall this box was pretty fun.  I enjoyed being able to read the blog post about the CVE discovery and actually employ it to compromise the integrity of a machine.
