# Plane Sight
<hr>

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/9ec201f2-7493-4c80-bd02-65aeb62022ad)

Download the file to your machine for analysis

Running the ```exiftool``` command you'll find something interesting

command:```exiftool mashle.jpg```

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/2c7d318f-4683-4ea6-bed3-fee75e755a64)

Similar to the "S1mple" chall you'll find out that what we have there is actually not a cipher, it is an encoding. Yeah, it's a cyrillic encoding

We can use this [webpage](https://2cyr.com/decode/?lang=en) to decode

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/2439320e-0de0-4604-971a-d5447b73797e)
![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/513bd7cf-6994-4b7d-addc-c657d7fe5140)

Well, that's a very weird output. Lets try to select a successful sample

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/5dd6b606-8afb-43be-9226-bc1f80c9ae38)

That looks less weird hehe, apparently it's not an encoding, it's a language. This means we can use google translator to translate this

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/67b33102-a036-4c62-9afd-517d4a731c55)

We still have to translate this

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/ebe10e1a-f2a3-4cd0-975f-e6f1a2c0247c)

Now, this makes more sense but then it didn't work when I tried ```ACTF{flag=17>>5_a11_1n_Th3_m3T4d473}```, we could have guessed this though to say ```it's all in the metadata``` so that'll be ```ACTF{flag=17'5_a11_1n_Th3_m3T4d473}``` but then it still didn't work.

Apparently I was meant to remove the ```flag=```

FLAG:-```ACTF{17'5_a11_1n_Th3_m3T4d473}```

Now, this is actually not the way to go😂, what if it is not a guessable word??

![image](https://github.com/BlackAnon22/AfricaCyberFest24_CTF/assets/67879936/de5b1a7e-9d45-42b3-a222-0f5305068867)

As you can see😂

That's why I had to use another cyrillic decoder, you can see it [here](https://convertcyrillic.com/#/)

After several tries, I got this

![image](https://github.com/BlackAnon22/BlackAnon22.github.io/assets/67879936/51d8a4a0-d011-46f7-9d58-a16d9754908d)

Yup, that's the proper way of solving it
