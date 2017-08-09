烧写预编译镜像
========================================

下载地址：
链接：http://pan.baidu.com/s/1i5002tJ 密码：6fhl
下载4个文件在同一个目录。

SD卡分区与格式化

.. sourcecode:: bash

    sudo  ./fsl-sdcard-partition.sh  -f imx6dl /dev/sdX
    # fsl-sdcard-partition.sh为分区脚本
    sudo mkfs.fat -n BOOT /dev/sdX1
    
关于在执行fsl-sdcard-partition.sh脚本时报错 sfdisk: unsupported unit 'M' ，原因是在高版本Ubuntu中，sfdisk不在支持参数-u,具体可以参见此文（https://community.nxp.com/docs/DOC-331316）中issue-4，修改方式有两种：

1.使用上文中文末给出的的下载链接，但要修改system分区的大小，至少要大于600M

2.直接使用原来的分区脚本，但是要去掉sfdisk的'-uM'参数，并且在每一个分区大小的后边直接加上M，示例如下：
.. sourcecode:: bash
    # 去掉 -uM
    
    sfdisk --force ${node} -N1 << EOF
    
    # 直接在分区大小后边加M指定大小
    
    ${BOOTLOAD_RESERVE}M,${BOOT_ROM_SIZE}M,83
    
    EOF

烧写uboot

.. sourcecode:: bash

    sudo dd if=u-boot170423.imx of=/dev/sdX bs=512 seek=2;sync

烧写boot.img

.. sourcecode:: bash

    cp boot.img /media/登录名/BOOT/boot.img

烧写system.img

.. sourcecode:: bash

    sudo dd if=system_raw170510.img of=/dev/sdX5;sync

uboot参数

.. sourcecode:: bash

    setenv bootargs 'console=ttymxc0,115200 init=/init video=mxcfb0:dev=ldb,bpp=32,if=RGB666 video=mxcfb1:dev=ldb,bpp=32,if=RGB666 video=mxcfb2:off video=mxcfb3:off vmalloc=320M androidboot.console=ttymxc0 consoleblank=0 androidboot.hardware=freescale cma=384M'
    setenv bootcmd 'fatload mmc 1:1 12000000 boot.img;boota 12000000'
    

