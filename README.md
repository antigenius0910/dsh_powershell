# dsh_powershell
Using dsh to control multiple Windows machines' powershell as the same time

Linux enviroment has a lots of very hands on and open source tools which we are familiar with, is there any way to manage Windows machine from Linux enviroment?

After research, I've found Distributed Shell with Bitvise SSH Server might just fit my needs.

First, we spin up two Win7 VMs.
![screen shot 2017-08-27 at 2 54 49 pm](https://user-images.githubusercontent.com/5915590/29803140-88e5ae60-8c3f-11e7-9006-0ce29cb424fb.png)

Second, we need to have ssh server running on our Win7 VMs, [Bitvise SSH Server](https://www.bitvise.com/ssh-server) seems like a great choice since it has the ability to let us gain a powershell in stead of cmd.exe. After install, we import our RSA.pub into Bitvise SSH Server as follow. 





, we follow [dsh - distributed shell on Mac OSX](http://michaelmasters.blogspot.com/2009/11/dsh-distributed-shell-on-mac-osx.html) and install dsh on my Mac. Make sure we have all the Win7 VM's ip in ~/.dsh/group/testMachines 
