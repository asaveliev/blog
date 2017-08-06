---
title: Comfortable Visual Studio Code and Raspberry Pi remote debugging
tags: vscode, raspberrypi, debug
---

> I am starting new home automation project with RPi, and will try to cover some interesting aspects of this development here.

I am using Raspberry, Kivy and Python to draw most parts of the UI. And of course I want to use my workstation to actually develop the code in Visual Studio Code, while using RPi touchscreen to interact with the app.

First step is to set up a comfortable dev enviroment - press F5, and code runs to the breakpoint. Sounds easy - RPi has good python environment, python tools for visual studio are available, remote debugging is supported... Not so fast! There are about 2 tutorials on the web on how to set this up, and they won't really cut it.

<!--break-->

## Launch.json
First step, which you'll find elsewhere, is to set up launch.json in VSCode (in .vscode subfolder of your code):

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach (Remote Debug)",
            "type": "python",
            "request": "attach",
            "localRoot": "${workspaceRoot}",
            "remoteRoot": "/home/sysop/myproject/",
            "port": 3000,
            "secret": "my_secret",
            "host": "192.168.11.32",
            "preLaunchTask": "DeployToRpi"
        }
    ]
}
```

You also have to modify your actual python code to wait until debugger connects. This must be on top of your main module:

```python
import ptvsd
ptvsd.enable_attach("my_secret", address = ('0.0.0.0', 3000))
ptvsd.wait_for_attach()
```

## Ready to go (?)
The button to start debugging shows up in VSCode, alas the code is not actually running on that 192.168.11.32 machine. You can of course copy it over there and run it manually - taking about 5 manual steps... not cutting it. So let's deploy!

You may have noticed the preLaunchTask in the above json. This is a "task" that vscode supports - these are normally run manually, but here we can run it before debugging starts. 

Make another file .vscode/tasks.json and here's my content:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "taskName": "DeployToRpi",
            "command": "${workspaceRoot}/deployandstart.bat",
            "type": "shell",
            "problemMatcher": []
        }
    ]
}
```

There are several commands in that bat file, so I made a file to keep them:

```bat
pscp -pw posys main.py sysop@192.168.11.32:/home/sysop/myproject/main.py
plink -pw posys sysop@192.168.11.32 "pkill -f python3"
plink -pw posys sysop@192.168.11.32 "cd myproject; nohup python3 main.py > /dev/null & "
```

Lines explanation:
1. Copy my file(s) to the RPi using secure copy
2. Kill previously running instance of python. Curiously, i couldn't add line 2 and 3 together - it killed the newly spawned python too, so i had to split them
3. Start my app, but redirect output and add "&" - plink won't close connection unless i do both.

Now these 3 lines took me a while to develop, due to interesting ways plink and bash interoperate. Note that this is of course on windows - similar can be done with regular ssh on other OSes, but on windows putty contains convinient file copy and remote execute shortcuts, which i am using here.

> sysop/posys are default name/password for Kivy installation on RPi. If you are not doing kivy, you will probably want to use pi/raspberry for Raspbian.


## Are we ready now?
If you followed previous 2 steps, press F5 and you should see terminal open, copy the code and ... hold on, can VSCode even debug python? It can not - so go to extension marketplace and get Python extension, which includes what we'll need.

On your RPi you'll have to install ptvsd (that's Python Tools for Visual Studio Debugger). 

_Make sure you do this_!
```
sudo pip install ptvsd==3.0.0
```

Installing without version number gets you 3.1 - which is currently not compatible with Python extension, giving you a great "Debug adapter process has terminated unexpectedly" error, with no explanation.

The reason is that protocol changed, but [VSCode extension hasn't been updated yet](https://github.com/DonJayamanne/pythonVSCode/issues/1018) . According to the bug, we'll have to stay on 3.0.0 until _next_ major version of ptvsd comes out, which will have reliable protocol.


## Now we are ready

Put a breakpoint on some code _after_ wait_for_attach line and hit F5!
Note that the startup process on RPi is obviously slower than your PC, so getting to some lines (especially under ```import``` lines)  may not be instant. 

Bonus - everything working!

[![screenshot](/images/IMG_20170806_153109_min.jpg)](/images/IMG_20170806_153109.jpg)
