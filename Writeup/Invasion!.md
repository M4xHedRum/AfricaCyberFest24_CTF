# Whispers in the Wires
<hr>

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/b76031c1-6181-4c65-91de-5de3f39fe2f0)

Download the pcap file

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/…/CTF/africa_cyberfest/forensics/whispers_in_the_wires]
└─$ ls -la
total 7828
drwxr-xr-x 2 bl4ck4non bl4ck4non    4096 May 20 12:15 .
drwxr-xr-x 7 bl4ck4non bl4ck4non    4096 May 20 17:39 ..
-rw-r--r-- 1 bl4ck4non bl4ck4non 8006936 May 19 15:27 ctf.pcapng
```
Running the ```strings``` command I found the string ```shadowheadquaters.com``` pop up multiple times

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/39743062-eb33-447c-a4cc-f67432bf5c4a)

So my teammate gave a one-liner command 

command:```tshark -r ctf.pcapng | grep shadowheadquarters.com | grep -v response | cut -d "A" -f 2 | cut -d "." -f 1 | xxd -r -p > abeg```

This command 

```
1. Extracts packets containing "shadowheadquarters.com"
2. Filters out response packets
3. Extracts the domain name
4. Saves it to "abeg" in raw binary format
```
Lets run the command

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/be38b358-0e44-4ea1-a99f-7ad1ac3bd31a)

Lets view that image

command:```open abeg```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/35869008-2170-42ef-bc26-9b25b2d07020)

Yup, thats our flag 

FLAG:-```ACTF{our_secrets_are_in_plain_sight!!}```

--------------------------------------

## Invasion!
<hr>

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/633b475d-9c04-4295-b3dd-ecc85e07e31d)

This is actually a huge file just so you know😅, download this file to your machine and unzip, you should see this

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/Downloads/CTF/africa_cyberfest/forensics]
└─$ cd disk_image      
                                                                                                                                                                                                                                             
┌──(bl4ck4non👽bl4ck4non-sec)-[~/…/CTF/africa_cyberfest/forensics/disk_image]
└─$ ls -la
total 18074244
drwxr-xr-x 2 bl4ck4non bl4ck4non        4096 May 19 04:08  .
drwxr-xr-x 3 bl4ck4non bl4ck4non        4096 May 19 07:11  ..
-rw------- 1 bl4ck4non bl4ck4non  9346220032 Apr 25 04:09 'doh ctf.vmdk'
```
We have a "vmdk" image, well what I did was convert this to a raw image using qemu

To install qemu you can use the command ```sudo apt-get install qemu-utils```

To convert to raw image you can use the command ```qemu-img convert -f vmdk -O raw doh\ ctf.vmdk doh.raw```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/d44c3002-3a68-4561-aa5d-0cd6dc84ce9b)

Now that we are done converting we can mount this using autopsy

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/50ec0517-8878-437d-a500-5afb7aa93450)

We'll be analyzing the ```C:\``` drive, that's where juicy stuff's at

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/d12bf5d3-433d-48aa-b13d-6a890f6564e9)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/d64d19c9-c63e-463a-8fb0-689c861603ed)

We'll do a file analysis

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/ec9a1226-3bf0-4f24-a5e8-e8b78a90aee0)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/5d6e8711-1940-4ca9-afd1-4655152d449b)

This user actually has something juicy in his Desktop directory hehe

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/a06667f3-4138-46c0-8de0-e222705f1d70)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/4a29c4dc-48c5-436b-be7e-2ce1e33dfaa2)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/08f4cd20-9c3c-47b8-9035-3ecb53d450b7)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/8b8284f2-a45d-4f78-ab6a-50c76fc16dd6)

We have this docx file, but then when you view the hex display you'll see that it has a different header

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/560175c3-4a7c-468f-9f9b-cdfa39082f89)

What's a PK header??

```
A PK header is a 4-byte sequence (50 4B 03 04) that identifies a file as a ZIP archive, serving as a file signature.
```
This means it's not really a docx file, rather it's a zip file.

Now this is what we'll do, we'll export this file to our machine then we change the extension from ```.docx``` to ```.zip```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/3864d736-9aae-42e7-8a4a-e26898c9ac5e)

Nice, now lets try to unzip

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/1d32ad7e-7292-4290-ba4e-a07ebf1ed81d)

You can see that a password is required, zip2joh won't work because the password isn't in rockyou😂

Lets go back to autopsy and check the ``` /Africa Cyberfest/``` directory

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/d04e41de-324f-4248-be91-aadaa5d6eb3f)

We can see that ```.DAT``` file

```
NTUSER.DAT is a Windows file storing user-specific settings and configuration data, including preferences, application settings, and account information, personalizing the Windows experience for each user.
```
Lets export this file too

If you run ```strings``` and ```grep``` you'll see this

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/5b3420a9-2e5e-4698-b031-522306d2c1a2)

This means our password's there actually

To extract data from this windows registry file we can use a tool ```reglookup```, you can use the ```sudo apt-get install reglookup``` to install the tool

command:```reglookup  NTUSER.DAT > abeg.txt```

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/Downloads/CTF/africa_cyberfest/forensics]
└─$ reglookup  NTUSER.DAT > abeg.txt
```
Now we can grep out the password from this txt file

command:```strings abeg.txt| grep "shadow_commander_password"```

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/Downloads/CTF/africa_cyberfest/forensics]
└─$ strings abeg.txt| grep "shadow_commander_password"
/Environment/shadow_commander_password,SZ,'%225dUiSm*4*m$A$',
```
Now there's a bit of a twist here, ```%22``` is the url encoded form of ```"```, it was the tool I used that actually url encoded it

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/0f406247-1677-4aa6-a2ad-845fa61676a3)

So we can say the password is ```"5dUiSm*4*m$A$```. we'll use this password to extract the zip file

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/42d7bc8c-3fbe-4f13-af94-cbe4c754095e)

This is what you get after you unzip the file, we have another zip file but then is this really a zip file??

command:```file shadow_document4.5.zip```

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/…/CTF/africa_cyberfest/forensics/shadow_document4.5]
└─$ file shadow_document4.5.zip 
shadow_document4.5.zip: CDFV2 Encrypted
```
Doing a little bit of research 

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/e222f143-c27e-41ea-9028-8f09c5511b87)

We can see that files like this do have the ```.docx``` extension, so we'll change the extension from ```.zip``` to ```.docx```

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/…/CTF/africa_cyberfest/forensics/shadow_document4.5]
└─$ ls -la                   
total 100
drwxr-xr-x 2 bl4ck4non bl4ck4non  4096 May 20 17:45 .
drwxr-xr-x 7 bl4ck4non bl4ck4non  4096 May 20 17:39 ..
-rw-r--r-- 1 bl4ck4non bl4ck4non 94208 Apr 20 18:57 shadow_document4.5.docx
```
Nice, now lets try to open this file

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/9fa08c7d-f545-424a-a5c2-e621e14d21b3)

oops, a password is required, we can reuse the password we got earlier

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/ddc40706-73a6-4c2d-88ed-d70bc9e0de5b)

We have this blank page, scrolling down we get this

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/51538213-5783-42fe-99fb-bd2aae013ac4)

But then when you use ```ctrl + A``` you'll see something hehe

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/e1d5490a-518e-4a14-a67f-7be71a29ddb0)

When you copy this and paste it into a normal text editor, you get this,

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/1ec73248-779d-4266-bbbf-d06f9fd48808)

But when you paste it into sublime text, you'll see that there's actually something there

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/6c8a12b8-0419-4934-8cf4-a38d13f3c86b)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/53a8a994-a377-4588-90aa-cb68676e263d)

Now, this has something to do with ```zero width joiner``` and we can decode this using this [website](https://330k.github.io/misc_tools/unicode_steganography.html)

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/9cea189c-415e-45dd-966e-9f1af7402d74)

We got our flag hehe

FLAG:-```ACTF{Sh4d0w_3xc3ut3d-haqhaq!}```

This chall just shows how messed up the brain of the creator is, bro's a maniac fr😂
