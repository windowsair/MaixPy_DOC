Edit and execute the file
=====

## MaixPy file system

MaixPy devices have an internal file system which can access both internal and external memories.  During boot, the device will mount any external memory cards formatted with either SPIFFS or FAT file systems, and add them to the internal file system as the '/flash' or '/sd' directories respectively.  

NOTES:

SPIFFS cards are by default assigned to `3MB` `SPIFFS` (starting at flash address `0xD00000`). When detected at boot, SPIFFS devices automatically appear as the `/flash` directory within the device's internal file system.  Currently the `SPIFFS` implementation in MaixPy does not support the creation of directories. 

FAT formatted SD (TF) cards are supported, but FAT32 or exFAT formatted cards are not currently supported.  When detected at boot, FAT formatted cards will be automatically mounted and appear as the `/sd` directory in the device's internal file system.  

It should be noted that the root directory is only used to mount the SD card or SPIFFS flash card. All other file operations will happen in the `/flash` or `/sd` directories, as deterimed by the format of the memory card inserted at boot time.  


## Navigating the file system

In MaixPy's REPL interface and in code, you may use the following commands.

`os.chdir()` changes the current directory, for example `os.chdir("/flash")`

`os.listdir()` lists the files in the current directory,


## Why do I want to edit and execute files?

To keep things simple in the previous [powering LED](led_blink.md) example, we entered code directly in the terminal at the Maix prompt, which was immediately executed upon entry. Such command line interfaces are often referred to as `REPL（Read Eval Print Loop）` where the the entered commands are immediately interpreted, and their results get displayed. MaixPy's REPL interface operates similar to other command line interfaces, except that the supported syntax is MicroPython.

While MaixPy's REPL interface is simple and convenient for small tasks, it soon becomes annoying to re-enter your code each time you want to run it. The answer is to save your code to a file, then execute the file.


## Edit and save files

### Way A: Edit by [Micropython Editor(pye)](https://github.com/robert-hh/Micropython-Editor) that integrated in maixpy

MaixPy includes a built-in open source editor [Micropython Editor(pye)](https://github.com/robert-hh/Micropython-Editor)

Use `pye("hello.py")` can create a file and enter the edit mode, keyboard shortcuts and other instructions can be found [here](https://github.com/robert-hh/Micropython-Editor/blob/master/Pyboard%20Editor.pdf)

Like we write code

```python
print("hello maixpy")
```

Then press the `Ctrl+S` press `Enter` button to save, and `Ctrl+Q` to exit the editor.

**Note** : The pye editor has certain requirements of the connected terminal. The `BackSpace` key should be sent as a `DEL`, otherwise the `BackSpace` will function as `Ctrl+H` (ie, character replacement).

Linux users are recommended to use `minicom`. Use `sudo minicom -s` to set the reference to [the previous tutorial](power_on.md)

Windows users often use the free terminal program PuTTY. Typing Shift-Backspace will cause PuTTY to send whichever code isn't configured here as the default.

Under Windows, too, according to their own use of the Internet search tool setting methods, such as `xshell` search `Xshell how to set backspace to del` to get results:

```
File → Properties → Terminal → Keyboard,
Change the delete and backspace sequences to ASCII 127.
```


### Way B: Read files to PC by [uPyLoader](https://github.com/BetaRavener/uPyLoader), then download to board after edit

[uPyLoader](https://github.com/BetaRavener/uPyLoader)

Download the executable: [release](https://github.com/BetaRavener/uPyLoader/releases)

![uPyLoader](../../assets/uPyLoader.png)

Select the serial port and click the `Connect` button to connect the board

The first time you run the software, you need to initialize it. Click `File->Init transfer files` to complete the initialization. This will create two files in the board, `__upload.py` and `__download.py`.

Then double click file name to read and edit, click `save` button to download file to board

### Way C: Read files to PC by [rshell](https://github.com/dhylands/rshell), edit, and then save back to board

Install [rshell](https://github.com/dhylands/rshell) fisrt according to the doc of `rshell`

```python
sudo apt-get install python3-pip
sudo pip3 install rshell
rshell -p /dev/ttyUSB1 # select board serial
```

Edit file

```python
ls /flash
edit /flash/boot.py
# the edit use vim
```


## Execution of documents


### Way A: `import`

Then execute `import hello`

You can see the output `hello maixpy`

But be attention, `import` command can only use once, if you want to execute code more than once, please use the `Way B`

### Way B: `exec()`

Use `exec()` to perform the functions

```python
with open("hello.py") as f:
    exec(f.read())

```

### Way C: Run in uPyLoader

Just select the file, then click `excute` button

### Way D: Directly run files on PC with ampy

[ampy](https://github.com/pycampers/ampy) 

run script by command `ampy run file_in_PC.py` to execute files on PC (file won't transmit to board)

