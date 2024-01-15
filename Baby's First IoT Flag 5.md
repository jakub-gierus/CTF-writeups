>   Part 5 - At [http://35.225.17.48:1234/firmware1.bin](http://35.225.17.48:1234/firmware1.bin) you will find the firmware. Extract the contents, find the hidden back door in the file that is the first process to run on Linux, connect to the backdoor, submit the password to get the flag. Submit the password to port 4545.


We are given the file firmware1.bin. When running the `file` command on it, we discover it is a gzip file. 

Using `gzip -d firmware1.gz` to extract the contents, we again use the `file` command on the extracted file. This time, the file is a `POSIX tar archive (GNU)`. 

First order of business is to extract the archive again, this time using tar: `tar -xvf firmware1`

The challenge prompt tells us to look for the first process to run on Linux. A likely directory for this is somewhere like /etc/init.d, which actually exists in the given system at /openwrt-x86-generic-generic-squashfs-combined.img.gz.extracted/0.extracted/squashfs-root/etc/init.d/, with a boot file in the directory.

In the boot file, the following comments at the top give us all the information needed: 

```
#Added for CTF
# Developer Password - Remove later
# Use the backdoor - IoTBackDoor
# On port 4545
```

Thus, the command `printf 'IoTBackDoor\n\0' | nc 35.225.17.48 4545` gives us the flag: {Develper_BackDoor}