Arch Linux 中文社区非官方生存手册
=========================================

:slug: archlinux_cn_community_unoffical_newbie_guide
:lang: zh
:date: 2015-01-28
:tags: Arch Linux,中文社区
:series: Arch Linux

.. PELICAN_BEGIN_SUMMARY

汝要入坑Arch Linux了吗?那还不赶紧加入Arch Linux中文社区~

.. PELICAN_END_SUMMARY

.. contents::

什么是Arch Linux?
-----------------------------

    Arch Linux 是一个针对 i686/x86-64 平台独立开发的 GNU/Linux 发行版，遵循轻量、简洁、优雅的开发原则，借灵活的架构应用于各种环境。Arch 安装后只提供最基本的系统，用户可以根据自己的需求来搭建不同的系统环境。官方并不提供图形化的配置工具，多数系统配置是通过修改文本文件来进行的。Arch 尽力提供最新稳定版本的软件。

    Arch Linux 使用 Pacman 作为包管理器，它在提供了一个简单的包管理器同时，也提供了一个易用的包构建系统，使用户能够轻松地管理和定制官方提供的、用户自己制作的、甚至是来自第三方的各种软件包。仓库系统也能够让用户轻松的构建和维护自己的编译脚本、软件包和仓库，这样将有助于社区的成长和建设。

    Arch Linux 的基本安装包由 [core] 软件库提供。此外 [extra], [community] 和 [testing] 软件库则提供了大量的的高品质软件以满足你的需求。Arch Linux 同时也通过 Arch 用户软件仓库(AUR)提供了 [unsupported] 软件库，里面有大量的编译脚本，用户可以通过 `makepkg` 工具轻松地从源码中编译软件。

    Arch Linux 采用“滚动升级”策略，这样可以实现“一次安装，永久更新”。升级到下一个“版本”的 Arch Linux 几乎不需要重新安装系统，只需一行命令，你就能轻松的享受到最新的 Arch Linux。

    Arch Linux 努力和上游软件源码保持一致，只有使程序能够在 Arch Linux 正常编译运行的补丁才会被加入更新中。

    总之， Arch Linux 是一个灵活、简洁的、满足有一定经验的 Linux® 用户的需求的发行版。它强大且易于管理的特性，使其成为可以完美胜任服务器和工作站的发行版。它可以变成任何你想要的样子。如果你也认为这是一个 GNU/Linux 发行版该做的，欢迎你来自由使用并参与其中，为社区做出贡献，欢迎来到 Arch Linux！

    --`Arch Linux 中文社区 <https://www.archlinuxcn.org/about/>`_ ,翻译自 `Archlinux.org <https://www.archlinux.org/about/>`_ . 

先把Arch Linux装上先~
-----------------------------

ArchWiki的文档应该算比较详细的啦~

* 第一次安装Arch Linux的新手建议看看 `Beginners' guide <https://wiki.archlinux.org/index.php/Beginners'_guide_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_

* 有经验的用户可以看看 `Installation guide <https://wiki.archlinux.org/index.php/Installation_guide_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_

* 想找交互式的安装程序? 可以试试 `ArchBoot <https://wiki.archlinux.org/index.php/Archboot>`_ 

加入Arch Linux中文社区论坛呗~
--------------------------------

用Arch Linux时发现了些问题?英文水平不足担心发到 `官方论坛 <https://bbs.archlinux.org>`_ 泥牛入海? 来中文论坛呗~

中文论坛在这~: `<https://bbs.archlinuxcn.org/>`_

和参加其他的论坛讨论一样,先读读 :fref:`phoenixlzx` 写的 `Arch Linux 中文社区 新手生存指南 <https://bbs.archlinuxcn.org/viewtopic.php?id=1072>`_ , `官方编写的论坛礼仪指南 <https://wiki.archlinux.org/index.php/Forum_etiquette_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_ 也可以作为参考呗~

加入中文社区的聊天频道呗~
--------------------------------

和论坛相比,中文社区聊天频道更 :ruby:`活跃|洪水` 呗~

在多才多艺的百合仙子 :fref:`lilydjwg` , Tox传教士 :fref:`quininer` ,和 PhD :fref:`farseerfc` 的协力下,社区交流群实现了irc+Telegram+xmpp+Tox的多通道联通,撒花~

-------------------------
加入irc频道
-------------------------

Web界面在这: :irc:`archlinux-cn`

如果汝使用irc客户端的话:
    
    irc服务器: :code:`irc.freenode.net`
    
    端口: :code:`7000` (SSL) / :code:`6667` (Plain)
    
-------------------------
通过XMPP加入
-------------------------

使用XMPP帐号添加 :code:`talk@archlinuxcn.org` 为好友即可加入。成功加入将收到欢迎信息。

-------------------------
通过Tox加入
-------------------------

添加下面那个Tox ID为好友,然后按照它的提示操作呗~

    34922396155AA49CE6845A2FE34A73208F6FCD
    6190D981B1DBBC816326F26C6CDF3581F697E7
    
------------------------
通过Telegram加入
------------------------

hmmm.....为保护群组不被外星人攻击，所以这里就不贴上链接啦~

汝可以通过其它方法加入,贴上汝在Telegram的用户名呗~(其它已经在群里的用户会帮汝拉进来......)

或者,在Telegram上添加@Jqs7Bot这个机器人,通过群组查询中的Linux分类找到#archlinux-cn(irc)的链接再加入呗~

-----------------------
群内的一般原则
-----------------------

* :del:`要优雅,不要污~`
* (irc/xmpp/tox) 推荐一个由百合仙子帮忙的图床 `<https://img.vim-cn.com/>`_ 呗~
* (Telegram) 发没压缩的图片和声音的话irc可是收不到的哟~


如果汝的英语水平也不错的话......
----------------------------------

如果汝的英语水平不错的话，太棒啦~,社区正需要汝这样的人呐~

汝可以......

* 帮助翻译ArchWiki,可以在 `ArchWiki上翻到相应页面呗~ <https://wiki.archlinux.org/index.php/ArchWiki_Translation_Team_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_

* 创建软件包并提交到 `AUR(Arch Linux User Repository) <https://wiki.archlinux.org/index.php/Arch_User_Repository>`_ ,高质量的软件包可能会被TU(授信用户)收录到官方软件仓库呗~

* `参与开发 <https://wiki.archlinux.org/index.php/Getting_involved>`_ , :del:`然后成为下一个像felixonmars一样的领袖😂😂😂`
