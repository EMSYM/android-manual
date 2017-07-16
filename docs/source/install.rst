烧写预编译镜像
========================================

下载地址：
链接：http://pan.baidu.com/s/1i5002tJ 密码：6fhl
下载4个文件在同一个目录。

SD卡分区与格式化

.. sourcecode:: bash

    sudo  ./fsl-sdcard-partition.sh  -f imx6dl /dev/sdX
    sudo mkfs.fat -n BOOT /dev/sdX1

烧写uboot

.. sourcecode:: bash

    sudo dd if=u-boot170423.imx of=/dev/sdX bs=512 seek=2;sync

烧写boot.img

.. sourcecode:: bash

    cp boot170510.img /media/marui/BOOT/boot.img

烧写system.img

.. sourcecode:: bash

    sudo dd if=system_raw170510.img of=/dev/sde5;sync

uboot参数

.. sourcecode:: bash

    setenv bootargs 'console=ttymxc0,115200 init=/init video=mxcfb0:dev=ldb,bpp=32,if=RGB666 video=mxcfb1:dev=ldb,bpp=32,if=RGB666 video=mxcfb2:off video=mxcfb3:off vmalloc=320M androidboot.console=ttymxc0 consoleblank=0 androidboot.hardware=freescale cma=384M'
    setenv bootcmd 'fatload mmc 1:1 12000000 boot.img;boota 12000000'
    

