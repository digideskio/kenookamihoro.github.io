凤凰卷家的 vps.to 的 OpenVZ VPS 试用小记
======================================================
:slug: vpsto_openvz
:date: 2016-02-22

.. PELICAN_BEGIN_SUMMARY

论感情牌+吐槽的必要性~（雾 

.. PELICAN_END_SUMMARY

.. contents::

凤凰卷( :fref:`phoenixlzx` )家的 `vps.to <http://vps.to>`_ [#]_ 最近新上线了 OpenVZ 架构的 VPS .卷说不会超售,咱相信她 (谁叫咱认为她是个好人咧~)

.. [#] 其实和喵窝一样,真正的大 boss 藏在后面~ (逃~

因为没人搭理她,她还在 :irc:`#archlinux-cn` 里吐槽了一番:

    [phoenixlzx] 没人理会透明卷

    [phoenixlzx] 还是滚去睡觉好了

    (quininer) 凤凰怎么了

    [phoenixlzx] 心情不好

    (quininer) momo

    [phoenixlzx] 当时要做的时候都说做做做做好了买买买

    [phoenixlzx] 然后现在做好了都不吱声了

    [phoenixlzx] 订单呢？订单呢？

    [phoenixlzx] 真是不靠谱
    
不过说到底还是在吐槽 OpenVZ 架构……

    
说到底还是在吐槽 OpenVZ 架构
-----------------------------------------

    OpenVZ是基于Linux内核和作业系统的操作系统级虚拟化技术。OpenVZ允许物理服务器运行多个操作系统，被称虚拟专用服务器（VPS，Virtual Private Server）或虚拟环境（VE，Virtual Environment）。

    与VMware这种虚拟机和Xen这种硬件辅助虚拟化技术相比，OpenVZ的主机与客户系统都必须是Linux（虽然在不同的虚拟环境里可以用不同的Linux发行版）。但是，OpenVZ声称这样做有性能上的优势。根据OpenVZ网站的说法，使用OpenVZ与使用独立的实体服务器相比，性能只会有1-3%的损失。

    OpenVZ是SWsoft, Inc.公司开发的专有软件Virtuozzo的基础。OpenVZ的授权为GPLv2。

    OpenVZ由两部分组成，一个经修改过的操作系统核心与一套用户工具。
    
    -- `Wikipedia:OpenVZ <https://zh.wikipedia.org/wiki/OpenVZ>`_
    
简单来说,OpenVZ 架构不是像 KVM Vmware 一类的完全虚拟化.而且 :ruby:`容易|几乎总是` 被服务商超售(不然咋会卖那么便宜😂😂).

不过由于和 :ruby:`主机|母鸡` 共享内核，听说会有性能优势？

购买时的一点吐槽
------------------------------------------

`链接在这,标准的 WHMCS 面板. <https://portal.vpsto.com/cart.php?gid=9>`_

可是为啥服务开通的确认邮件被 Yandex Mail 当作 spam 了啊 (ノ=Д=)ノ┻━┻

除了这个其它还OK,VPS 的管理界面也是标准的 SolusVM 面板。

:del:`真的是太标准了好像连主题都没改😂😂`

另外没有咱喜欢的 Arch Linux 还是很遗憾呐~(卷把锅甩给了OpenVZ,因为母鸡定制的内核太老 (2.6.x) 😂😂

顺便提一下这次咱买的是最便宜的那个(一个月也要99😂😂),配置大概像这样:

    1 Core CPU

    1 GB Guaranteed RAM

    25 GB HDD

    400 GB Premium Traffic @ 100Mbps Bandwidth

    SoftLayer Hong Kong Datacenter

日常
----------------------------------------

刚开通时的系统是 CentOS 6 , :del:`吓的咱赶紧换成了 Debian 8`,VPS 控制面板上就重装系统,大概一两分钟左右吧.

登录,先 apt update 一下,连的好像是 Debian 官方的仓库,速度不够快.

.. code-block:: bash

    # apt update
    .....
    Fetched 528 kB in 5s (94.2 kB/s)               

接下来更新系统,速度在 2M/s 左右,还可以(懒得换镜像源了😂😂

.. code-block:: bash

    root@s17931102:~# apt full-upgrade
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    Calculating upgrade... Done
    The following NEW packages will be installed:
        e2fsprogs init libss2
    The following packages will be upgraded:
        cpio libc-bin libc6 libgcrypt20 locales multiarch-support
    6 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
    Need to get 11.5 MB of archives.
    After this operation, 3327 kB of additional disk space will be used.
    Do you want to continue? [Y/n] 
    ...
    Fetched 11.5 MB in 4s (2705 kB/s)
    ...

胡乱的性能测试
-------------------------------

`咱照着这篇文章小小的测试了一下😂😂 <http://www.freehao123.com/vps-cpu-io-unixbench/>`_

CPU 是 Intel(R) Xeon(R) CPU E5-2650 v3 @ 2.30GHz .

.. code-block:: bash

    # cat /proc/cpuinfo
    processor	: 0
    vendor_id	: GenuineIntel
    cpu family	: 6
    model		: 63
    model name	: Intel(R) Xeon(R) CPU E5-2650 v3 @ 2.30GHz
    stepping	: 2
    microcode	: 45
    cpu MHz		: 2300.033
    cache size	: 25600 KB
    ......

:code:`free -m` 了一下:

.. code-block:: bash

    root@s17931102:~# free -m
                 total       used       free     shared    buffers     cached
    Mem:          1024        315        708         14          0        292
    -/+ buffers/cache:         23       1000
    Swap:            0          0          0

Debian 8 Minimal,还没有装任何软件时大概用掉了23M 内存.

:code:`dd` 了两下,速度不错:

.. code-block:: bash

    root@s17931102:~# dd if=/dev/zero of=test bs=64k count=4k oflag=dsync
    4096+0 records in
    4096+0 records out
    268435456 bytes (268 MB) copied, 1.08052 s, 248 MB/s
    root@s17931102:~# dd if=/dev/zero of=test bs=8k count=256k conv=fdatasync
    262144+0 records in
    262144+0 records out
    2147483648 bytes (2.1 GB) copied, 2.51345 s, 854 MB/s

下载一个来自 Cachefly 的测速文件,差不多跑满了 100Mb 的带宽:

.. code-block:: bash

    root@s17931102:~# wget http://cachefly.cachefly.net/100mb.test
    Resolving cachefly.cachefly.net (cachefly.cachefly.net)... 205.234.175.175
    Connecting to cachefly.cachefly.net (cachefly.cachefly.net)|205.234.175.175|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 104857600 (100M) [application/octet-stream]
    Saving to: '100mb.test'
    
    100mb.test                    100%[===================================================>] 100.00M  11.7MB/s   in 9.0s   
    
    2016-02-22 02:52:54 (11.1 MB/s) - '100mb.test' saved [104857600/104857600]
    

从 :fref:`cuihao` 的镜像源下载 Arch Linux 的安装映像,也能跑满百兆带宽:

.. code-block:: bash

    root@s17931102:~# wget https://mirrors.ustc.edu.cn/archlinux/iso/2016.02.01/archlinux-2016.02.01-dual.iso
    Resolving mirrors.ustc.edu.cn (mirrors.ustc.edu.cn)... 202.141.176.110, 2001:da8:d800:95::110
    Connecting to mirrors.ustc.edu.cn (mirrors.ustc.edu.cn)|202.141.176.110|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 735051776 (701M) [application/octet-stream]
    Saving to: 'archlinux-2016.02.01-dual.iso'

    archlinux-2016.02.01-dual.iso 100%[===================================================>] 701.00M  11.7MB/s   in 63s    

    2016-02-22 02:57:09 (11.2 MB/s) - 'archlinux-2016.02.01-dual.iso' saved [735051776/735051776]

由于没装web服务器所以先不测试出口带宽......

运行了一个小脚本来测速,噫可赛艇~:

http://freevps.us/downloads/bench.sh

.. code-block:: bash

    Speedtest (IPv4 only)
    ---------------------
    Your public IPv4 is foo
    
    Location		Provider	Speed
    CDN			Cachefly	11.1MB/s
    
    Atlanta, GA, US		Coloat		7.26MB/s 
    Dallas, TX, US		Softlayer	8.13MB/s 
    Seattle, WA, US		Softlayer	10.6MB/s 
    San Jose, CA, US	Softlayer	9.82MB/s 
    Washington, DC, US	Softlayer 	9.11MB/s 
    
    Tokyo, Japan		Linode		11.2MB/s 
    Singapore 		Softlayer	11.5MB/s 
    
    Rotterdam, Netherlands	id3.net		6.91MB/s
    Haarlem, Netherlands	Leaseweb	7.45MB/s 
    
综上所述,网速棒棒哒~

编译来自 `lnmp.org <http://lnmp.org>`_ 的LNMP 一键安装包......

用了40多分钟,算不算快咧 _(:з」∠)_ 

一点总结
-------------------

总之卷的 VPS 还是很棒呐~(๑•̀ㅂ•́)و✧，不过真的还是有些贵😂😂，对于只想搭个梯子的人来讲花销有些大 (\´･ω･\`).

:del:`为了接着用 Arch Linux,咱还是回去用 conoha 吧 _(:з」∠)_`

