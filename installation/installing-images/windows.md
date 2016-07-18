# Installing operating system images using Windows

On Windows you have the choice of the command line `dd` tool or using the graphical tool `Win32DiskImager` to write the image to your SD card. Please note that these tools can overwrite any partition of your machine. If you specify the wrong device in the instructions below, you could delete your primary Windows partition, and/or your documents. Please be careful.

## Graphical option (`Win32DiskImager`)

- Insert the SD card into your SD card reader and check which drive letter was assigned. You can easily see the drive letter, such as `G:`, by looking in the left column of Windows Explorer. You can use the SD card slot if you have one, or a cheap SD adapter in a USB port.

- Download the Win32DiskImager utility from the [Sourceforge Project page](http://sourceforge.net/projects/win32diskimager/) and install the program.

- Start the program; you may need to run this as administrator. To do so, right-click on the program, and select **Run as administrator**.

- Select the image file you extracted earlier.

- Select the drive letter of the SD card in the device box. Be careful to select the correct drive; if you get the wrong one you can destroy the data on your computer's hard disk! If you are using an SD card slot in your computer and can't see the drive in the Win32DiskImager window, try using an external SD adapter.

- Click `Write` and wait for the write to complete.

- Exit the imager and eject the SD card.

## Command Line with Git-Bash

- Install Git and Git-Bash from [the Git website](https://git-scm.com/download/win) (you can use default settings when installing)

- Find Git-Bash in your start menu, right-click on the file, and select **Run as administrator**.

- For the following commands, note that your `C:\` drive is under `/c/` and Git-Bash uses forward-slashes, never back-slashes.

- Run `cat /proc/partitions` to see what devices are currently connected.

- If your computer has a slot for SD cards, insert the card. If not, insert the card into an SD card reader, then connect the reader to your computer.

- Run `cat /proc/partitions` again. The new device that has appeared is your SD card (verify it matches the drive letter in your file browser). The `name` column gives the device name of your SD card; it will be listed as something like `mmcblk0p1` or `sdd1`. The last part (`p1` or `1` respectively) is the partition number but you want to write to the whole SD card, not just one partition. You therefore need to remove that part from the name and add `/dev/` to the front, getting, for example, `/dev/mmcblk0` or `/dev/sdd` as the device name for the whole SD card. Note that the SD card will show up more than once in the output of `cat /proc/partitions`; the first entry will be the whole drive (e.g. `sdd`), and the second and subsequent entries will be partitions (`sdd1`, `sdd2` and so on) - you will get more than 1 partition if you have previously written a Raspberry Pi image to this SD card, because the Raspberry Pi SD images have more than one partition.

- In the terminal, write the image to the card with the command below, making sure you replace the input file `if=` argument with the path to your `.img` file, and the `/dev/sdd` in the output file `of=` argument with the right device name. This is very important, as **you will lose all data on the hard drive if you provide the wrong device name**. Make sure the device name is the name of the whole SD card as described above, not just a partition of it; for example, `sdd`, not `sdd1`, and `mmcblk0`, not `mmcblk0p1`.

    ```bash
    dd bs=4M if=/c/Users/Name/Downloads/2016-05-27-raspbian-jessie.img of=/dev/sdd
    ```

- Please note that block size set to `4M` will work most of the time; if not, please try `1M`, although this will take considerably longer.

- Also note that if you do not run Git-Bash as Administrator, you will get an error similar to `dd: failed to open '/dev/sdd': Permission denied`.

- The `dd` command does not give any information of its progress and so may appear to have frozen; it could take more than five minutes to finish writing to the card. If your card reader has an LED it may blink during the write process. To see the progress you can add `status=progress` to the `dd` command. Your command would then look like this:

    ```bash
    dd bs=4M status=progress if=/c/Users/Name/Downloads/2016-05-27-raspbian-jessie.img of=/dev/sdd
    ```
If you get the error `dd: invalid status flag: ‘progress’`, then you are using an old version of `dd`, and this is not supported (it will exit immediately without writing to your SD card).

- Once complete, eject the card via the normal process. The card will probably show garbage data until you eject it; this is perfectly normal.

---

*This article uses content from the eLinux wiki page [RPi_Easy_SD_Card_Setup](http://elinux.org/RPi_Easy_SD_Card_Setup), which is shared under the [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses/by-sa/3.0/)*
