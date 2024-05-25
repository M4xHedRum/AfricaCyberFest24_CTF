### Whispers in the Wires

We are given a pcap file and when I opened it in Wireshark I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/1ce96d0c-72aa-44c4-b930-07949287b83f)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/3d576ea6-5f72-484a-9f8f-9e08c2e101fd)

First thing I tried was to get an idea of the protocol hierarchy
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/11d73676-ecc5-41b7-8a67-821fef935c72)

There's HTTP protocol but after checking it I didn't see anything of relevance there

I actually spent lot of time working on this but it was an easy challenge once you just figure it out

I saw that there were various DNS protocol and it looked weird
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/61c95759-a425-4a64-b128-1433608dd230)

The domain queried first was `89504e470d0a1a0a0000000d49484452000002c10000019008060000004f.shadowheadquarters.com`

At this point I knew it was using DNS exfiltration because that's the header of a png file

Incase you don't know what DNS exfiltration is, it's basically a means by which attackers/red-teamers exfiltrates data using the dns protocol

So what we need to do now is to extract all the values from the dns queried then convert it from hex

Actually during the ctf I extracted it manually by using `strings` and some `match & replace magic` but my teammate used a one linear which made life easier
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/c141be71-916c-41ed-bd85-f46eb7b4d70d)

```
tshark -r ctf.pcapng | grep shadowheadquarters.com | grep -v response | cut -d "A" -f 2 | cut -d "." -f 1 | xxd -r -p > lol.png
```

Running that command would decode the image file
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/89ad53b9-0c6a-4c39-9f3a-6fe8ae56b87b)

When we open it we get this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/002326da-07fa-4991-a95a-36f5ec1d1c19)

Just a blue pane image

I went over to [aperisolve](https://www.aperisolve.com/) and got the flag in the `view->superimposed` pane
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/46c02db4-e826-41b4-b40f-efd205c004be)

```
Flag: ACTF{our_secrets_are_in_plain_sight!!}
```
