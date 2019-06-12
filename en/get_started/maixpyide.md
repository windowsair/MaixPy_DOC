MaixPy IDE
=======

![helloworld-run.png](./assets/helloworld-run.png)

`MaixPy` uses the `Micropython` syntax, and unlike other languages like `C`, it doesn't need to be compiled. In fact, you don't even need the IDE to use it, you can use the serial terminal.

Using the `IDE` will facilitate real-time editing of the script on the computer, as well as being able to view the camera images in real time, and save files to the development board.

Of course, using the `IDE` will take some resources from the board (for the transmission and debugging), however the performance is not noticeably affected.

## MaixPy firmware

To use the `MaixPy IDE` , the firmware must be at least `v0.3.1`, otherwise it won't connect. Check the firmware and IDE version before using it, to ensure normal operation.

## Download the installation package

[dl.sipeed.com](http://dl.sipeed.com/MAIX/MaixPy/ide/)

Check the latest version by reading the `readme.txt` file.

## Installation

#### Using the installer (**Recommended**, simple and convenient)

For `Windows` run the installer directly by double-clicking the file. For `Linux` you will need to give the file permission and execute it using the command line:

```
chmod +x maixpy-ide-linux-x86_64-0.2.2.run
./maixpy-ide-linux-x86_64-0.2.2.run
```

#### Using a compressed package (`7z`)

Extract the file to a folder

> If your system does not support `7z`， you will need to [download `7z`](https://www.7-zip.org/) and install it.

If on `Linux`, you can use the terminal to decompress the file:

```bash
sudo apt install p7zip-full
7z x maixpy-ide-linux-x86_64-0.2.2-installer-archive.7z -r -omaixpy-ide
# `-o` is immediately followed by the decompressed path, there is no space in between.
```

* After decompression, execute:
  * On `Windows`： Double-click `maixpyide` to execute. You might want to right-click and pin it to the taskbar or start menu.
  * On `Linux`： Use the following commands:
```
chmod +x setup.sh
./setup.sh
./bin/maipyide.sh
```

## Test run

Open the IDE and select the model number of the development board in the upper toolbar.

`Tool-> Select Board`

Click on `connect` to make a connection with the `MaixPy IDE`

![connect-icon.png](./assets/connect-icon.png)

After the connection is successful, the button will turn from green to red.

![connect-success.png](./assets/connect-success.png)

Below the connection button is the `Run` button, which will execute the current `py` file opened in the editor.

![helloworld-run.png](./assets/helloworld-run.png)

Click the `Run` button again to stop the execution.

## Uploading files

You will find ways to upload files in the `Tool` dropdown menu.

## Note

Only open one serial port connection at a time, make sure to close the previous connection before opening a new one.
