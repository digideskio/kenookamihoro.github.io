Systemd-boot 小试
===========================================

:slug: bootctl_simpleguide
:lang: zh
:date: 2015-01-18 09:35
:tags: Arch Linux,Systemd

.. PELICAN_BEGIN_SUMMARY

systemd 是 Linux 下的一款系统和服务管理器，兼容 SysV 和 LSB 的启动脚本。systemd 的特性有：支持并行化任务；同时采用 socket 式与 D-Bus 总线式激活服务；按需启动守护进程（daemon）；利用 Linux 的 cgroups 监视进程；支持快照和系统恢复；维护挂载点和自动挂载点；各服务间基于依赖关系进行精密控制。

不过最近Systemd越做越大，从启动管理到连接网络的功能都有了，说不定哪一天咱们就能看到Systemd-linux了呢（雾）

.. PELICAN_END_SUMMARY

.. contents::

Systemd-boot到底是啥？
-------------------------

    systemd-boot (以前被称为gummiboot) 是可以执行 EFI 镜像文件的简单 UEFI 启动管理器。启动的内容可以通过一个配置(glob)或者屏幕菜单选择。systemd 从版本 220-2 开始包含此组件。

    配置很简单，但是只能启动 EFI 可执行程序，例如 Linux 内核 EFISTUB, UEFI Shell, GRUB, Windows Boot Manager等。

    -- `ArchWiki:Systemd-boot <https://wiki.archlinux.org/index.php/Systemd-boot_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>`_

简单来说,Systemd-boot是一个适用于UEFI的轻型启动管理器呗~

用Systemd-boot的要求是啥?
--------------------------------

.. alert-warning::

    Systemd-boot 可以用来启动 `EFISTUB <https://wiki.archlinux.org/index.php/EFISTUB>`_ 内核 ,如果汝在启动EFISTUB内核时遇到了 `像这样的问题 <https://bugs.archlinux.org/task/33745>`_ ,那么汝就只好换个不使用EFISTUB的启动管理器了(例如GRUB)呐~

简单来说，Systemd-boot的要求只有......

    1.确认启动方式是 UEFI 模式
    2.验证可以正确访问 EFI 变量
    3.EFI 系统分区正确挂载，而且内核和 initramfs 已经被复制到 ESP。 (因为systemd-boot 没办法从其它分区加载 EFI 程序呢, :del:`真是个大笨驴......`)

一般来说只要汝的硬盘上有GPT分区表和一个EFI 系统分区应该就没问题了呗~

安装Systemd-boot
------------------------------

首先要把EFI系统分区挂载上(Arch Linux的规范和Systemd-boot的要求是挂载到 :code:`/boot` 上),然后运行安装命令:

.. code-block:: bash

    sudo bootctl --path=/boot install

汝认为这样就结束啦?不对,还有配置工作等着汝呐~

下面的设置过程中咱就用$esp来代替EFI系统分区的挂载点了呗~

基本设置
------------------------------

Systemd-的基本设置保存在 :code:`$esp/loader/loader.conf` 中呐~,用文字编辑器创建一个
