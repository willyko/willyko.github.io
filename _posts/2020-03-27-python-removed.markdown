---
layout: post
title: "Ubuntu Disaster - Story of Removed Python3"
date: 2020-03-27 01:00:00 -0800
categories: virtualmachine python ubuntu
comments: true
---

Ever since learning about linux system, I've always been told not to do `sudo rm -rf /` as that single command will break down your system.
What happened today was similar to that and I shall tell you the story

Let me tell you the setup I have first.  I have VirtualBox 5.2.34, with MacOS host and Ubuntu 18.04.4 guest. 

I was fiddling around with git repo that requires python3.7 so I had to install python3.7 via `sudo apt-get install python3.7`. Then I realize the package wants me to have python3.7 linked as python3, so after some research, I saw two ways to do it.

1. Use update-alternatives
```
sudo apt-get install python3.7
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 2
sudo update-alternatives --config python3
```

2. Use pyenv

Recalling that using version env can get hectic sometimes I decided to give the first way a try,
and that was probably my mistake.  It turns out my python3.7 starts to mess around with my environment,
and I was getting python error in my terminal when I enter commands that don't exist.
I didn't screen cap it but the error was something like apt_pkg not found instead of the normal "command not found"
I did notice that doing a `sudo update-alternatives --config python3` and changing it back to python 3.6 fix that issue

# Mistake
At one point I decided that why not just move over to python3.7 completely so I ran the following command
```
sudo apt-get remove python3
```
THIS IS A BIG MISTAKE.  DO NOT DO THIS

The result? Everything starts to break, and I mean it, everything.
Since that command took awhile to run, I went to grab a coffee and came back to the login screen.
The entry no longer works, eg cannot type anything into the password prompt.
I quickly rebooted the VM, and the whole GUI (ubuntu-desktop) doesn't start anymore, and I was stuck with a commandline prompt.
Logging into the console, I can see that I no longer has internet access (`apt-get update` and `ping` no longer work). 

# Recovery
Digging through stackoverflow and ubuntu forums, it looks like uninstalling python3 does exactly just that; breaks your system
So I found a post to recover using Live CD.  Since I am using VirtualBox, this is probably the easiest to do.
Shut down the box, open the setting for VirtualBox, change the drive to LiveCD and start the VM. 

This is the post I used to get the mounting to work [askubuntu post](from askubuntu[https://askubuntu.com/questions/985288/how-to-restore-a-virtual-system-after-accidentally-removing-all-kernels])  from step 4 to step 11.
At step 9, I also did a `sudo apt-get install -reinstall ubuntu-desktop`
After this, I was able to remove the LiveCD and launch Ubuntu's desktop env again.

# More Issue
Happily seeing the desktop loading, I noticed an issue right away.  The VM does not have access to the internet.
In fact, the top right corner doesn't even show the internet connection icon.  When checking the Setting, there's no wired connection section, only VPN and proxy

# More Solution
Back to googling through posts and forums, I tried multiple methods and I did end up getting to work.  In all honesty, I'm not sure if 
every step is required, but I will just share what I tried

First one I'm pretty sure didn't do the job. [superuser post](https://superuser.com/questions/648396/virtualbox-ubuntu-can-not-connect-to-internet)
There were no `8.8.8.8` and `8.8.4.4` nameserver entries in `/etc/resolv.conf`, but adding them and restarting network-manager didn't seem to change anything

Then I tried this [askubuntu post](https://askubuntu.com/questions/1049302/wired-ethernet-not-working-ubuntu-18-04)
`sudo lshw -C` showed that my network card is disabled, so I enabled it by adding it to 
`sudo vi /etc/network/Interfaces`  and then turned it on 
`sudo ifup enp3s0`
This did enable the network icon at the top right corner of my desktop.  However, the connection is still off and I cannot connect to the internet

Next, I tried this [askubuntu post](https://askubuntu.com/questions/1088953/ubuntu-18-04-missing-wired-connections-in-settings)
And this is what actually got the connection to suddenly go up
```
sudo touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
sudo systemctl restart NetworkManager
```

# Conclusion
With the desktop recovered, internet backup, I did one more `sudo apt-get install -reinstall python3 ubuntu-desktop`
Everything seems to work like before, and I sincerely hopeful there's no deeper damage to my system.

Bottom Line: DO NOT REMOVE PYTHON3 
