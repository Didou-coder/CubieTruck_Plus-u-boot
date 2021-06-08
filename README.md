
**This is a simple guide to build uboot for Cubietruck Plus**

# source code 

If you want to use u-boot on SD card, make sure 
"boot_normal=fatload mmc 0 40007800 uimage;bootm 40007800\0" \

If you want to use u-boot on eMMC, make sure 
"boot_normal=fatload mmc 2 40007800 uimage;bootm 40007800\0" \

in u-boot-2011.09/include/configs/sun8iw6p1.h

See https://github.com/cubieboard/Cubietruck_Plus-u-boot/blob/master/u-boot-2011.09/include/configs/sun8iw6p1.h#L331


# build step:

  ./build.sh -p sun8iw6p1




# update uboot:

After build , You will get uboot binaries, There is two way to update uboot using the build binaries.

* Way 1:

Refer: https://github.com/cubieboard/Cubietruck_Plus-binaries

You need to copy the build binaries to corresponding directory:

  Cubietruck_Plus-u-boot/u-boot-2011.09/boot0_sdcard_sun8iw6p1.bin  --> Cubietruck_Plus-binares/bin/raw/u-boot-spl-sun8iw6p1.bin<br>


  For SD card
  Cubietruck_Plus-u-boot/u-boot-2011.09/u-boot-sun8iw6p1.bin        --> Cubietruck_Plus-binares/bin/raw/u-boot-sun8iw6p1.bin 
	
  For eMMC
  Cubietruck_Plus-u-boot/u-boot-2011.09/u-boot-sun8iw6p1.bin        --> Cubietruck_Plus-binares/bin/raw/u-boot-sun8iw6p1-card2.bin 

* Way 2: (this way is used for assuming you have booting system in Cubietruck Plus)

1. copy the build binaries to Cubietruck Plus system

  Cubietruck_Plus-u-boot/u-boot-2011.09/boot0_sdcard_sun8iw6p1.bin  -->  /root/boot-file/u-boot-spl-sun8iw6p1.bin
  For SD card
  Cubietruck_Plus-u-boot/u-boot-2011.09/u-boot-sun8iw6p1.bin        -->  /root/boot-file/u-boot-sun8iw6p1.bin
  For eMMC
  Cubietruck_Plus-u-boot/u-boot-2011.09/u-boot-sun8iw6p1.bin        -->  /root/boot-file/u-boot-sun8iw6p1-card2.bin


2. Update uboot in Cubietruck Plus system

  $ cd /root/boot-file
  
  if you are booting from tfcard, run:<br>
  $ sudo ./update_sys_config.sh tfcard

  if you are booting form emmc, run:<br>
  $ sudo ./update_sys_config.sh emmc

More details see :

https://github.com/cubieboard/Cubietruck_Plus-products/tree/master/cb5/cb5-linaro-desktop-hdmi/overlay/root/boot-file
https://github.com/cubieboard/Cubietruck_Plus-products/blob/master/cb5/cb5-linaro-desktop-hdmi/overlay/root/boot-file/update_sys_config.sh

# rootfs run on SSD or HDD 

Format SSD or HDD,then copy rootfs into disk.
Change mmc_root=/dev/mmcblk0p2 to mmc_root=/dev/sda1 and add rootdelay=5 setting in u-boot-2011.09/include/configs/sun8iw6p1.h.


-       "mmc_root=/dev/mmcblk0p2\0" \
+       "mmc_root=/dev/sda1\0" \
        "loglevel=8\0" \
-       "setargs_mmc=setenv bootargs console=${console1} console=${console} root=${mmc_root} " \
+       "setargs_mmc=setenv bootargs console=${console1} rootdelay=5 console=${console} root=${mmc_root} " \


Rebuild source code and update u-boot binaries. 
