
### Finding nulock 
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6d7d2808-87fc-427a-adf1-14d8a71ddec2)

This was the first reverse engineering challenge, and we were given an apk file
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/879edf55-dc4a-4cd5-8def-d02c99aa7e90)

First thing I tried was to unzip the file, grep for the flag, convert the dex files to jar using `dex2jar`, decompile the converted dex file using `jd-gui`

Doing that I didn't really get anything and `jd-gui` decompilation was a bit off

So I tried using an online [decompiler](https://www.decompiler.com/)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/f27807bc-1b9d-4e35-a29c-171ecd8bda4d)

Next i downloaded the zip file
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/e1d2da91-b134-4e92-8595-7f5f5890b7a0)

Unzipping it and opening in vscode should give this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/0546674f-029b-4b31-8f79-5af3e04c2914)

Looking through the classes i saw `Payload.java` which looked interesting

Viewing it shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/c00628aa-dd8a-4be1-b408-27c9ba2a6e08)

We can right away tell we should decode that

I just copied and paste that array to python interpreter then converted them to `chr`
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6c7ef4ee-319b-439e-9c1a-2f6ccf069e71)

That gives error and that's because it isn't in the printable range for example `-5` isn't a printable value

To fix this we need to `AND` it with `255 == 0xff` which is equivalent to `% 256` and that would make each value there in the printable range
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/b7423d26-bc55-47aa-8305-6756837dd86b)

With that we get the flag

```
Flag: ACTF{Dynamic_Analysis_h0s7_R3v3al5}
```
