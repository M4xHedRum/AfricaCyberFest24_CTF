
### Sore
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/d984a085-ec6b-4941-9aba-e33f9ac2aa9d)

The second and last reverse engineering challenge

We are given an executable file
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/5d6a3fa3-f782-4524-9779-14b2aa55e964)

The description states that it's a malware yet silly me still ran it 💀

When I ran it, the program asks for input
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/b3526d2b-d8e4-4072-9b1f-ed772c7d80e5)

After we give it input then it would log out from the current session

That's why i am running it in gdb so that it won't logout yet

One important thing is this:

```
Input the flag. I'll let you know if it's correct
```

The program claims it will let us know if our provided input is right? That means it's going to be giving us an oracle which would basically allow us know if each character of the given input is right or wrong therefore giving us the primitive to brute force the flag

But before we think of brute forcing I needed a way to prevent it from logging out

I threw the binary into Ghidra to do some reversing or so I thought?

Unfortunately the binary is a rust compiled binary and I am not familiar with rust so I had some issue with figuring out what it does

But my goal was to figure out the instruction which would call program to logout our session and thereby patching it

Starting from the entry function I got this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/23910358-607c-48f0-9196-38a4edf7e3ae)

The main function is the first parameter passed to `__libc_start_main` which is `FUN_0012bc90`

Clicking on it shows this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/9e243555-5daa-49ca-bbd3-d6086776ec73)

```c
void FUN_0012bc90(int param_1,undefined8 param_2)

{
  code *local_8;
  
  local_8 = FUN_0012b890;
  FUN_002eb380(&local_8,&PTR_FUN_00398048,(long)param_1,param_2,0);
  return;
}
```

Variable `local_8` is a function pointer to `FUN_0012b890` 

Clicking on that shows this, which seems to be the function that handles the input validation and presumably the logout function?
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/6284df13-42c8-4dde-9227-d6af6224c464)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/113d03b0-3b29-4943-b0f1-6564116a0459)
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/22ff1f66-8298-4609-aa79-ddd32651c8f3)

Since it's going to logout after we give it input i decided to start clicking on functions which is at the end

```c
LAB_0012baba:
    local_90 = &PTR_s_Wrong!_Input_the_flag._I'll_let_y_00398098;
    local_88 = 1;
    local_80 = 
    "Wrong!\nInput the flag. I\'ll let you know if it\'s correct\nFailed to read inputmain.rsYou\'re  partially right\n"
    ;
    local_78 = ZEXT816(0);
    FUN_002eee30(&local_90);
    local_90 = (undefined **)thunk_FUN_00167990();
    if (local_90 != (undefined **)0x0) {
      FUN_0012b750(&local_90);
    }
    goto LAB_0012bb04;
  }
  goto LAB_0012ba50;
}

```

On clicking funtion `thunk_FUN_00167990`, i saw that it's the function that handles the logout
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/2067c8e3-d4db-4a28-9f18-f957f8c8bcd0)

The function names are stripped which makes assumption hard since I don't know rust but we can tell because of this:

```c
cVar6 = FUN_001671e0("org.gnome.SessionManager/org/gnome/SessionManagerorg.kde.ksmserver/KSMServer org.kde.KSMServerInterfacelogoutorg.xfce.SessionManager/org/xfce/SessionManagerorg.freedesktop.log in1/org/freedesktop/login1org.freedesktop.login1.ManagerLogout"
```

The logout string there should just make you guess you are at the right track (don't quote me) 👀

Now that we figured that we need to patch the opcode
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/2d9f8845-6e10-457a-aa53-eb2eccf8240d)

```
        0012baea ff 15 58        CALL       qword ptr [->thunk_FUN_00167990]                 undefined thunk_FUN_00167990()
                 d3 27 00                                                                    = 0012de50
```

So we will change `0xff1558d32700` to `0x909090909090`

With that instead of the program calling that function it will just do nothing (nop -> no operation)

Here's the [script](https://github.com/h4ckyou/h4ckyou.github.io/blob/main/posts/ctf/cyberfest24/scripts/sore/patch.py) I wrote to patch it

```python
with open("sore", "rb") as f:
    binary = f.read()

f.close()

binary = binary.replace(b"\xff\x15\x58\xd3\x27\x00", b'\x90'*6)

with open("patched", "wb") as f:
    f.write(binary)
```

Running that it should patch the binary and now we can easily run it

To confirm if the program would do what it says I tried this
![image](https://github.com/h4ckyou/h4ckyou.github.io/assets/127159644/95b6d519-d5a7-4f02-8c0c-12055927db06)

Luckily it wasn't a bluff and now we can brute force

Here's the [script](https://github.com/h4ckyou/h4ckyou.github.io/blob/main/posts/ctf/cyberfest24/scripts/sore/solve.py) I wrote to achieve that

```python
import string
import subprocess

flag = ""
charset = string.ascii_letters + '_{}'

while True:
    for char in charset:
        command = f"echo {flag+char} | ./patched"
        execute = subprocess.Popen(command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
        output, err = execute.communicate()
        print(f"Trying {flag+char}")

        if 'Wrong' not in output:
            flag += char
            break
    
    if flag[-1] == "}":
        break

print(f"FLAG: {flag}")
```

Running it works and I got the flag
[![asciicast](https://asciinema.org/a/MaYoHe5yASPW95v3Nm4DDvFiu.svg)](https://asciinema.org/a/MaYoHe5yASPW95v3Nm4DDvFiu)

```
Flag: ACTF{xor_xor_diff}
```
