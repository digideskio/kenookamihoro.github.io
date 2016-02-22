装完 Arch Linux 再装 Windows 然后修引导~
======================================================

:slug: windows_grub_recuse
:lang: zh
:date: 2016-02-22 17:00
:tags: windows,grub,凑数
:series: Arch Linux

.. PELICAN_BEGIN_SUMMARY

装完 Arch Linux 再装 Windows 以后 GRUB 没啦~ Windows 出来背锅 _(:з」∠)_ 

.. PELICAN_END_SUMMARY

.. contents::

:del:`要啥 Windows 啊~`

准备工作
-----------------------------

* 一个可启动的 Linux 的 Live USB ( 咱是用的 Arch Linux 的安装 ISO )

好像没啥了诶~(最好要连上网,可以参阅 `ArchWiki <https://wiki.archlinux.org/index.php/Beginners'_guide>`_ 呗~ ).

如果汝使用 UEFI 主板，且 UEFI 启动模式（优于 BIOS/Legacy 模式）已启用，CD/USB 会自动通过systemd-boot 启动 Arch Linux。要确认是否已进入UEFI模式，检查下面目录是否有文件呗~：

    # ls /sys/firmware/efi/efivars

确定设备名称,然后挂载
-----------------------------

用 :code:`lsblk` 确定汝 Linux 安装到哪个磁盘里呐~

    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    
    sda         8:0    0 298.1G  0 disk 
    
    ├─sda1    8:1    0    40G  0 part 
    
    ├─sda2    8:2    0 256.1G  0 part 
    
    └─sda3    8:3    0     2G  0 part 
    
咱这个栗子是 :code:`/dev/sda1` ,MBR 模式的. :code:`sda1` 是 :code:`/` ,:code:`sda2` 是 :code:`/home` (\´･ω･\`).

然后挂载上, archiso 里有个天然的适合的挂载点~

    # mount /dev/sda1 /mnt
    
    # mount /dev/sda2 /mnt/home
    
chrooting......
----------------------------------

用 :code:`arch-chroot` chroot 进目标系统:

    # arch-chroot /mnt /bin/bash
    
MBR  安装 GRUB (\´･ω･\`)
--------------------------------

这样(记得用汝实际的磁盘名称替换 :code:`sda` ,不要后面的数字.)

    # grub-install --target=i386-pc --recheck --debug /dev/sda
    
    # grub-mkconfig -o /boot/grub/grub.cfg


UEFI 安装 GRUB _(:з」∠)_  
-----------------------------------

这样(某些系统的 :code:`--efi-directory` 可能是 :code`/boot/EFI` ,不管啦 (ノ=Д=)ノ┻━┻ )

    # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub --recheck
    
    # grub-mkconfig -o /boot/grub/grub.cfg
    
-------------------------------

最后离开 chroot 环境然后重启,记得拔掉U盘~

    # exit
    
    # reboot
    
所以嘛,要啥 Windows 呐~ (ノ=Д=)ノ┻━┻
