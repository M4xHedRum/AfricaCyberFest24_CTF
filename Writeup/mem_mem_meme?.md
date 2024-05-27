# mem mem meme?
<hr>

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/3d1e676b-07c9-4aa8-8cdd-565ceb1a4d0b)

Download this file to your machine and unzip

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/fe33134d-0f8f-4324-853f-041fd8ce5327)

I'll actually be needing volatility for this, but damnnn I've been finding it difficult to install

Now that I've installed volatility, lets cook

To install volatility, you can get it [here](https://github.com/volatilityfoundation/volatility3)

I actually solved this chall the unintended way hehe

We'll start out by checking the OS and kernel details of the memory sample we want to analyze using the ```windows.info.Info``` plugin

command:```python3 vol.py -f ../../../../Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/challenge.vmem windows.info.Info```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/f6f72884-ce10-4613-af0e-9c63c613f39d)

From the above screenshot, we can see the kernel name and also the layer name

Using the ```windows.pslist.PsList``` plugin, we can list the processes present in the windows memory image

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/7c71d257-4cd2-4064-a0e5-2f9d1abc3edd)

command:```python3 vol.py -f ../../../../Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/challenge.vmem windows.pslist.PsList```

We can see that there's a ```notepad.exe``` process running with a pid of ```3064```

To get more details about these process we can use the ```windows.cmdline.CmdLine``` plugin to list the process command line arguments

command:```python3 vol.py -f ../../../../Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/challenge.vmem windows.cmdline.CmdLine```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/c2088771-2bd0-46d0-8faa-34e05edc7b4a)

Now this is more detailed, the first time we checked the proccess running we found a ```notepad.exe``` process with a pid of ```3064```, but then we can see from the above screenshot that this proccess has the args ```"C:\Windows\system32\NOTEPAD.EXE" \\172.16.56.1\share\ip.txt```. We can also see that there's another ```notepad.exe``` process with a pid of ```3044``` and that it has the arg ```"C:\Windows\system32\NOTEPAD.EXE" C:\Users\Crash Override again\Desktop\password.txt```. In summary we can say PID 3044 is a Notepad process that has opened the file "password.txt" located on the desktop of the user "Crash Override again".

One thing we can do here is try to dump the process, to do this I actually didn't use volatility3, I used volatility2 and this is because of the ```memdump``` plugin. You can get volatility2 [here](https://github.com/volatilityfoundation/volatility)

To use volatility2 we'll need the memory profile, we can get this using the ```imageinfo``` plugin

command:```python2 vol.py -f ../../../../Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/challenge.vmem imageinfo```

```
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/bl4ck4non/Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme?/challenge.vmem)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf80002a510a0L
          Number of Processors : 2
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002a52d00L
                KPCR for CPU 1 : 0xfffff880009ef000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2024-04-25 07:59:23 UTC+0000
     Image local date and time : 2024-04-25 08:59:23 +0100
```
You should get that output, we have different profiles here, lets go with this profile ```Win7SP1x64```

Now that we've goten the profile lets use the memdump plugin to help us dump the ```notepad.exe``` process

command:```python2 vol.py -f ../../../../Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/challenge.vmem --profile=Win7SP1x64 memdump --dump-dir=/home/bl4ck4non/Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/ -p 3044```

```--dump-dir```  specifies the directory where the dumped memory will be saved, in this case, a directory named "/home/bl4ck4non/Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/"

```-p``` specifies the PID of the process for which to extract the memory dump

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/Documents/Tools/forensics/volatility]
└─$ python2 vol.py -f ../../../../Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\?/challenge.vmem --profile=Win7SP1x64 memdump --dump-dir=/home/bl4ck4non/Downloads/CTF/africa_cyberfest/forensics/mem_mem_meme\? -p 3044  
Volatility Foundation Volatility Framework 2.6.1
************************************************************************
Writing notepad.exe [  3044] to 3044.dmp
```
Nice, now lets get our flag

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/…/CTF/africa_cyberfest/forensics/mem_mem_meme?]
└─$ ls -la 3044.dmp          
-rw-r--r-- 1 bl4ck4non bl4ck4non 209444864 May 25 17:18 3044.dmp
                                                                                                                                                                                                                   
┌──(bl4ck4non👽bl4ck4non-sec)-[~/…/CTF/africa_cyberfest/forensics/mem_mem_meme?]
└─$ file 3044.dmp              
3044.dmp: Windows Event Trace Log
```
All that's left is to grep the flag out

command:```strings 3044.dmp | grep -i "actf"```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/ca1cc2df-3f29-4bd3-8bd6-da2d61c26805)

Yup, that's our flag

FLAG:-```ACTF{Sh4d0w_1nc1d3nt_C0mp1ic4t10n}```
