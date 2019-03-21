Edit and execute the file
=====


## MaixPy has a built-in file system

Support Flash to use `SPIFFS` (not currently support creating directories), by default assigned to `3MB` `SPIFFS` ( start at flash address `0xD00000`), boot automatically hang on to `/flash` the directory

Also supports the FAT format SD (TF) card, boot automatically linked in to `/sd` the directory

It should be noted that the root directory is only used to mount the SD card or Flash, specific file `/flash` or `/sd` directory

## Why do I need to edit and execute the file?

In the front [powering LED](led_blink.md) experiment, we performed a one directly inside the terminal entering code, so that also simple, we will enter the command executed immediately and promptly returns the data, this interaction is called `REPL（Read Eval Print Loop：Interactive interpreter）`, in this way The advantage is that it is simple and convenient, and it is very similar to the Linux terminal, except that the syntax used is replaced by the MaixPy (Micropython) syntax.

But in most cases, we would like to write the script to a file and then execute the file, so that we don't have to type the code every time, which reduces a lot of trouble.


## Edit and save the file

### Way A: Edit by [Micropython Editor(pye)](https://github.com/robert-hh/Micropython-Editor) that integrated in maixpy

In MaixPy, we have a built-in open source editor [Micropython Editor(pye)](https://github.com/robert-hh/Micropython-Editor)

Use `os.listdir()` can view the files in the current directory,

Use `pye("hello.py")` can create a file and enter the edit mode, keyboard shortcuts and other instructions can be found [here](https://github.com/robert-hh/Micropython-Editor/blob/master/Pyboard%20Editor.pdf)

Like we write code

```python
print("hello maixpy")
```

Then press the `Ctrl+S` press `Enter` button to save, according to `Ctrl+Q` exit edit

**Note** : Use this editor has certain requirements for the use of serial tool, you must be `BackSpace` key as a `DEL` function, or else the `BackSpace` call is `Ctrl+H` the same function (ie, character replacement).

Linux recommended use `minicom`, you need `sudo minicom -s` to set the reference to [the previous tutorial](power_on.md)

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


## Execution documents

Use `os.chdir()` Change directory to the directory of the current file, such as `os.chdir("/flash")`

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

### Way D: Run with ampy

[ampy](https://github.com/pycampers/ampy) 

run script by command `ampy run file_in_PC.py`

