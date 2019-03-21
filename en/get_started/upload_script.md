Upload script to development board
======

Earlier we came across the `pye("filename.py")` command to open an editor that directly edits files in the file system.

But slowly we will find that this method is only suitable for changing a small amount of code. When the amount of code is huge or we need to highlight support, it does not apply. We need to write the code on the computer and upload it to the board. Inside the file system

There are currently several methods:

## Uploading and running scripts using the graphical tool uPyLoader

[uPyLoader](https://github.com/BetaRavener/uPyLoader) is an open source software that allows you to easily connect to MaixPy and upload, download, and execute files, monitor output, and more.

Download the executable: [release](https://github.com/BetaRavener/uPyLoader/releases)

![uPyLoader](../../assets/uPyLoader.png)

Select the serial port and click the `Connect` button to connect the board

The first time you run the software, you need to initialize it. Click `File->Init transfer files` to complete the initialization. This will create two files in the board, `__upload.py` and `__download.py`.

On the left, select the file you want to upload and click `Transfer` to upload it to the board's file system.

On the right is the file inside the board, click `List files` to refresh the file list, select the file name, click `Execute` to execute the script file.

Click on `View -> terminal ` above to open the terminal to view the runtime output or send a command

## Using Tool rshell

正如使用 `linux` 终端一样， 使用 [rshell](https://github.com/dhylands/rshell) 的 `cp` 命令即可简单地复制文件到开发板

Just like Linux terminal do, use [rshell](https://github.com/dhylands/rshell) to copy files to board, use `cp` command~

Install rshell first

```python
sudo apt-get install python3-pip
sudo pip3 install rshell
rshell -p /dev/ttyUSB1 # 这里根据实际情况选择串口
```

Then copy file~

```python
ls /flash
cp ./test.py /flash/ #复制电脑当前目录的文件 test.py 到开发板 flash 根目录
```

More about rshell: [rshell on github](https://github.com/dhylands/rshell)



## Using the command line tool ampy

[ampy](https://github.com/pycampers/ampy) is an easy-to-use command line tool for uploading, downloading, and executing files, and open source

Note that this tool is running on the computer, not on the board.

Use `ampy --help` to view help information

Using the `ampy run file_in_PC.py` command also allows you to run the script directly on the board without uploading the script to the board.


## TF card copy

After copying to the TF card, execute `import filename` in the terminal to run the script.
