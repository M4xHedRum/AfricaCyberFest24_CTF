### S1mple
![image](images/simpler.png)



I downloaded the simple.txt file and upon opening it, I noticed whitespace encoding beneath the binary content. Subsequently, I employed the stegsnow tool in my Kali Linux environment to proceed with analysis.
![image](images/simple.png)


I cross-checked it on my Kali Linux environment, then used the file command on the simple.txt file to verify if it was indeed a text file. Afterward, I ran the strings command on the simple.txt file and obtained the same output.
![image](images/kali.png)

Then I employed the stegsnow tool with the -C simple.txt command to scrutinize the image.
![image](images/kali2.png)

If stegsnow isn't installed, you can install it using the appropriate command for your system."
To install stegsnow on Kali Linux, you can use the following command in your terminal:


sudo apt-get install stegsnow


I then copied the cipher to the website mashke.org, which provides an automatic Cyrillic encoder. I tried several encodings, but when I used the Windows-1251 encoding, I successfully obtained the flag, which was translated from Russian to English.
![image](images/mask.png)


I converted it Boom! I got the flag.
![image](images/mashke6.png)
```
Flag: ACTF{1ts_ju57_a5_5impl3_4s_1t_100k5!!!}
```
=======
# S1mple

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/abe6d93b-5683-46d2-80ac-2ee26c947ba7)

Lets download the file and check the content

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/bcd3a44c-12ea-4d85-8f4e-87c962ad3685)

That looks like binary but then that's a lot of space, well from experience I can tell that I'm meant to use ```stegsnow``` to solve this hehe

To install this tool you can use the command ```sudo apt install stegsnow```

To use the tool

command:```stegsnow -C simple.txt```

```
┌──(bl4ck4non👽bl4ck4non-sec)-[~/Downloads/CTF/africa_cyberfest/cryptography]
└─$ stegsnow -C simple.txt
ÀCÒÔ{1òñ_jó57_à5_5èìïë3_4ñ_1ò_100ê5!!!}
```
Now, that's some weird cipher, that's what I thought at first😂. 

But then, this is actually not a cipher hehe, it is an encoding, it's called a cyrillic encoding

We can decode this using this [webpage](https://2cyr.com/decode/?lang=en)

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/61622b5d-3ed6-481a-a82c-3a2d1505eb02)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/d74a9a5b-500e-41ac-91aa-befa6dd66aa1)

You can see we have some weird output, now lets select the "source encoding" and also the "displayed as" option

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/ae71ea43-8e24-47cc-8861-8b308db9f9b8)

Now, we have a more resonable output

I was actually stuck here for a while because I thought this guy ```АCТФ{1тс_jу57_а5_5импл3_4с_1т_100к5!!!}``` was an encoding but then I thought wrong, it's actually a language lool.

We can translate this using google translator

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/3d1a34c7-b72c-4007-a79b-7fc3b7c514d0)

Yup, that's our flag hehe

FLAG:-```ACTF {1ts_ju57_a5_5impl3_4s_1t_100k5!!!}```

