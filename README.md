# dsh_powershell
Using dsh to control multiple Windows machines' powershell as the same time

Linux enviroment has a lots of very hands on and open source tools which we are familiar with, is there any way to manage Windows machine from Linux enviroment?

After research, I've found Distributed Shell with Bitvise SSH Server might just fit my needs.

First, we spin up two Win7 VMs.

![screen shot 2017-08-27 at 2 54 49 pm](https://user-images.githubusercontent.com/5915590/29803140-88e5ae60-8c3f-11e7-9006-0ce29cb424fb.png)

Second, we need to have ssh server running on our Win7 VMs, [Bitvise SSH Server](https://www.bitvise.com/ssh-server) seems like a great choice since it has the ability to let us gain a powershell in stead of cmd.exe. After install, we import our RSA.pub into Bitvise SSH Server as follow. 

![screen shot 2017-08-28 at 10 34 22 pm](https://user-images.githubusercontent.com/5915590/29803413-3ee21fae-8c41-11e7-985c-b52afc1a2c44.png)

Third, we follow [dsh - distributed shell on Mac OSX](http://michaelmasters.blogspot.com/2009/11/dsh-distributed-shell-on-mac-osx.html) and install dsh on my Mac. Make sure we have all the Win7 VM's ip in ~/.dsh/group/testMachines. 

```bash
bash-3.2# cat ~/.dsh/group/testMachines
192.168.133.21
192.168.133.22
```

For Distributed Shell you can add different group later on (e.g., NFS server group, workstation group, Domain controller group...) in order to separately control your servers group by group. By using dsh we can then quicky submit Linux command to Linux server farm and Windows command to Windows server farm without interference with each other.

Finally, both Distributed Shell with Bitvise SSH Server are in place then we could submit some powershell commmand to control or monitor multiple windows servers.

e.g. Get error and warning entries from all remote computer on a certain day and order by the source

```bash
bash-3.2# dsh -g testMachines 'Get-EventLog -LogName System -After "08/27/2017" -Before "08/29/2017" | Where-Object {$_.EntryType -like "Error" -or $_.EntryType -like "Warning"} | Sort-Object Source '
192.168.133.21: 
192.168.133.21:    Index Time          EntryType   Source                 InstanceID Message   
192.168.133.21:    ----- ----          ---------   ------                 ---------- -------   
192.168.133.21:     1313 Aug 28 21:38  Error       BTHUSB                 3221553158 The add...
192.168.133.21:     1314 Aug 28 21:38  Error       BTHUSB                 3221553169 The loc...
192.168.133.21: 
192.168.133.21: 
192.168.133.22: 
192.168.133.22:    Index Time          EntryType   Source                 InstanceID Message   
192.168.133.22:    ----- ----          ---------   ------                 ---------- -------   
192.168.133.22:      883 Aug 28 20:40  Error       bowser                 3221233475 The mas...
192.168.133.22:     1190 Aug 28 21:38  Error       BTHUSB                 3221553169 The loc...
192.168.133.22:     1189 Aug 28 21:38  Error       BTHUSB                 3221553158 The add...
192.168.133.22:      322 Aug 27 13:52  Warning     Microsoft-Windows...          219 The des...
192.168.133.22:      822 Aug 27 19:16  Warning     Tcpip                  2147487876 TCP/IP ...
192.168.133.22: 
192.168.133.22: 

```

We can also mix windows powershell command with liunx command together.

```bash
bash-3.2# dsh -g testMachines 'Get-WmiObject -Class Win32_ComputerSystem -Property UserName -ComputerName .' | grep -i UserName
192.168.133.21: UserName         : yen-PC1\yen
192.168.133.22: UserName         : yen-PC2\yen
```

In conclusion, now we are able to control multiple Windows machines through powershell and mix it with Linux commands in order to 
maximise deployment and monitoring time.
