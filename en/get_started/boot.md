Boot script
=======

If a memory card formatted with either a SPIFFS or FAT file system is detected at boot, MaixPy will look for a `boot.py` file in the root directory of the memory card.  If such a file is found, the system will immediately execute it.  If the `/boot.py` file is not found, the system will automatically create a simple one on the memory card.  Edit the contents of `/boot.py` file on the memory card to determine what the device does when reset or rebooted.

Notes: 
  1.  Memory cards must be formatted with a SPIFFS or FAT filesystem, not FAT32.
  2.  FAT memory cards will have a mount point of `/sd`, while SPIFFS formatted cards appear as `/flash`.
