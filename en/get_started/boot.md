Boot script
=======

If a memory card formatted with either a SPIFFS or FAT file system is detected at boot, MaixPy will look for a `boot.py` file in the root directory of that memory card.   If `/boot.py/ is found, the system will immediately execute it. 

If the `/boot.py` file is not found the device will automatically create a default one on the memory card that offers a [REPL command line interface](./edit_file.md).  Edit the contents of the `/boot.py` file on the memory card to determine what the device does when it is reset or rebooted.

Notes: 
  1.  Memory cards must be formatted with a SPIFFS or FAT filesystem, not FAT32.
  2.  FAT memory cards will have a mount point of `/sd`, while SPIFFS cards will appear as `/flash`.
