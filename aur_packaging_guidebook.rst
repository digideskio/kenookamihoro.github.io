AUR 纯萌新向入门教学(2)-创建一个软件包
===================================================

:slug: aur_packaging_guidebook
:lang: zh
:date: 2015-02-03
:tags: Arch Linux,AUR,Guides
:series: Arch Linux

.. PELICAN_BEGIN_SUMMARY

上一次咱说了 `「从AUR中安装软件包」 </aur_fresh_guidebook.html>`_ ,其实如果汝足够 :del:`触` 的话,不妨自己创建个软件包呗~

.. PELICAN_END_SUMMARY

.. contents::

首先为啥不读读ArchWiki咧?
-----------------------------------------------------------------

:del:`ArchWiki ,短小精悍,汝值得拥有呐~`

* `创建软件包 <https://wiki.archlinux.org/index.php/Creating_packages_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_

* `PKGBUILD <https://wiki.archlinux.org/index.php/PKGBUILD_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_

* `Arch Linux 打包标准 <https://wiki.archlinux.org/index.php/Arch_packaging_standards_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>`_

如果因为各种原因看了Wiki还不明白的话,接着往下看呗~
  
第一步:以普通的方式安装软件
-----------------------------------------------------------------

看看 :code:`base-devel` 装了没?

.. code-block:: bash
    
    sudo pacman -S base-devel --needed

从上游把软件的源代码下载下来,按照上游的文档编译和安装(典型的例子像这样):

.. code-block:: bash
    
   ./configure
   make
   make install

如果汝为了顺利安装做了任何的调整(比如改了些源码或者打上了补丁),记下来操作步骤,待会儿编写PKGBUILD时要用到哟~


第二步:编写PKGBUILD文件
-------------------------------------------------

    PKGBUILD是一个shell脚本,包含 Arch Linux 在构建软件包时需要的信息.

    Arch Linux 用 makepkg 创建软件包.当 makepkg 运行时,它会在当前目录寻找 PKGBUILD 文件,并依照其中的指令去获取依赖文件,编译出 pkgname.pkg.tar.xz 文件.生成的包内有二进制文件和安装指令,可以使用 pacman 进行安装.

    pkgname,pkgver,pkgrel和arch是必须包含的变量.license在构建包时并不强制要求,但若要分享 PKGBUILD文件,推荐加上该变量,否则 makepkg 会有警告. 

    -- `ArchWiki:PKGBUILD <https://wiki.archlinux.org/index.php/PKGBUILD_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29>`_

---------------------
获得原型
---------------------

ArchWiki上关于PKGBUILD的介绍已经很详细啦~,从 :code:`/usr/share/pacman/` 找个合适的原型复制下来:

* PKGBUILD.proto       (经典原型😂)
* PKGBUILD-vcs.proto   (如果汝的源码来自像SubVersion,Git,Mercurial一类的 :ruby:`SCCS|源代码控制系统` 的话,看下这份原型呗~)
* PKGBUILD-split.proto (如果汝要做一个分包的话)
* proto.install        (希望在安装之前/之后运行一些别的命令?看看这份原型呗~)

阅读原型上的注释,然后删掉(随汝心意啦,但是如果汝想上传软件包的话,[Maintainer/偶尔会有的Contributor]是必须的😂)

--------------------
起个名字
--------------------

软件包的名字保存在pkgname里,只能用小写字母、数字和@ . _ + - 这些字符，且不允许用.或者-作开头。

别和 AUR 或官方仓库里面的软件包重名了哟~

--------------------
挑个许可协议
--------------------

为了不造轮子,传送门在此~ `<https://wiki.archlinux.org/index.php/PKGBUILD_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#license>`_

---------------------
编译和安装时的命令
---------------------

makepkg的运行顺序大概像这样(从上到下):

* 获得,解压和检查源代码的散列值.

* pkgver():在汝的源代码来自各种SCCS时会有用,用来更新软件包的版本号.

    `ArchWiki:VCS_package_guidelines <https://wiki.archlinux.org/index.php/VCS_package_guidelines#The_pkgver.28.29_function>`_ 有一些范例,可以看看呗~
    
    不过记得得给 :code:`pkgver` 变量随便赋个值先......
    
* prepare():一些预处理源文件以进行构建的命令,比如打补丁......

    把汝为了让源代码顺利编译而运行的操作加到这里.不过首先要切换到源码目录呗~
    
    如果汝在编译前不需要干任何事情,这个函数可以不用.
    
* build():真正 :del:`撸起袖子` 开始编译软件包的过程.
    
    如果汝的软件包啥都不用编译,这个函数可以不用.
    
    对于普通的configure-make-make install三部曲来说,build()可以写成这样(汝来决定那些注释的去留呗~)
    
    .. code-block:: bash
    
        # 切换到源码目录
        cd "$srcdir/$pkgname-$pkgver"
        # configure 和 make ,按照Arch Linux的规范,软件包都装在/usr目录
        # 汝也许要按照上游的指示添加别的参数呐~
        ./configure --prefix=/usr
        make
    
    不要在这个函数中让用户进行交互,见 `某个bug报告 <https://bugs.archlinux.org/task/13214>`_

* check():用来执行make check和其他一些例行测试的地方,有时需要.

* package():把生成的文件打包成软件包的函数, **只有这个函数是必须的.** 

    pkg目录复制了根目录下软件安装路径的继承关系.
    如果汝需要手动把文件放到根目录下,那么在这里你需要把文件放在pkg下相同的文件层级结构中诶~.
    比如,把一个文件安装到/usr/bin,那么在伪root环境中对应的路径为$pkgdir/usr/bin.
    
    对于普通的configure-make-make install三部曲来说,package()可以写成这样:
    
    .. code-block:: bash
    
        make DESTDIR="$pkgdir/" install
        
    在一些很罕见的情况下,软件只有安装在单一目录下时才能运行.在这种情况下汝还是老老实实把它安装到$pkgdir/opt下吧.

    通常,软件在安装过程中会在pkg目录下先创建一系列子目录.如果没有的话,makepkg会报错,记得先在build()函数中提前手动创建这些目录哟.
    
    同build(),不要在这个函数中让用户进行交互.

--------------------------------
安装前后有事要做?
--------------------------------

如果汝要在安装/升级/卸载前后运行其它命令,可以写个.install文件:

* pre_install - 安装前运行的脚本,可以传递版本号为参数.
* post_install - 安装后运行的脚本,可以传递版本号为参数.
* pre_upgrade - 升级前运行的脚本,可以按新版本号,旧版本号的顺序传递参数.
* post_upgrade - 升级后运行的脚本,可以按新版本号,旧版本号的顺序传递参数.
* pre_remove - 卸载前运行的脚本,可以传递版本号为参数.
* post_remove - 卸载后运行的脚本,可以传递版本号为参数.

这些函数运行的也是Bash脚本哦~     

然后在PKGBUILD中把 :code:`install` 变量指向汝的 :code:`.install` 文件的位置呗~

.. code-block:: bash

    # 一般来说,.install的文件名应该和软件包名一致.
    install='foo.install'
    
-------------------------------
需要用到配置文件?
-------------------------------

如果汝的软件包要有些用户编写的配置文件,记得添加到backups变量里.

例如如果汝的配置文件是 :code:`/etc/foo` :

.. code-block:: bash

    backup=(etc/foo)
    
记得是相对于 :code:`/` 的路径.

这样pacman就会在软件包升级时提醒用户合并新的和旧的文件,在卸载软件包时这些文件会被保留(除非用了 :code:`pacman -Rn` )

---------------------------------
依赖,依赖,依赖!
---------------------------------

:del:`重要的事情说三遍--`

架构相关的变量可以通过下划线加架构的方式指定：depends_x86_64=(), optdepends_x86_64=().

依赖相关的变量有这些:

* depends: **真** 运行时依赖

    运行时 **必须** 的软件包列表,可以使用比较运算符来描述版本限制,如：depends=('foobar>=1.8.0').
    
    不过如果A依赖B,B又依赖C的话,A的depend里不用加上C😂😂
    
* optdepends:运行时的可选依赖

    一组不影响软件主要功能,但能提供额外特性的软件包.应该简要说明每个包所能提供的额外功能.有些可选依赖如果不安装,软件包的个别程序可能无法正常使用. 
    
    optdepends可以这样写:
    
    .. code-block :: bash
    
        optdepends=(
            'foo: some description'
        )
    
    尽可能给每个可选依赖一个简洁的描述来方便用户决定装不装~
    
* makedepends:只在编译时需要的依赖

    仅在软件编译时需要的软件包列表.可以像depends序列里提到的一样指定最小版本依赖. 
    
    不过不要包含 :del:`base-devel` 组的软件包! (要运行makepkg的话这个软件包组应该已经装上了)
    
* checkdepends:只在测试时需要的依赖

    运行测试组件时需要,而运行时不需要的包列表.和makedepends一样,不要包含 :del:`base-devel` 组的软件包.
    
    只有编写了check()时再填这个变量哟~
    
makedepends和checkdepends中的软件包会因为makepkg的 -r 选项而在安装完成后删除.

-------------------------
散列值:安全第一
-------------------------

记得加上文件的散列值,makepkg会在编译前检查文件的散列值(和PKGBUILD中的散列值比较),一定程度上确保源代码不会篡改.

写法大概像这样:

.. code-block :: bash

    {这里是汝选择的散列算法}=('{散列值}')
    
散列值的顺序取决于汝的sources变量,例如如果汝选择sha512sum的话:

.. code-block :: bash

    sha512sums=(".....") 
    
建议使用sha256sums(或更高的位数),md5已经发现有 `碰撞漏洞 <https://en.wikipedia.org/wiki/MD5>`_ ,sha1已经发现Preimage漏洞(已知校验和的情况下，可以生成一段字符串产生相同的校验和,)

如果汝的源代码来自SCCS的话,因为文件在不断变化,所以需要让makepkg跳过散列值检查:
    
.. code-block :: bash

    sha512sums=("SKIP")

记得在修改某个文件以后用新的散列值这个变量呗~


测试,测试,再测试
------------------------------------

.. alert-info ::

    :del:`如果只是汝自己用的话，就不必做这个质量保证了，因为只有汝一个人需要忍受这些错误呗~.`
    
运行下makepkg命令来确保没有问题。如果PKGBUILD没有错误，将会生成一个包，但是如果PKGBUILD被破坏或未完成，它会抛出一个错误。

如果运行makepkg 成功，会生成一个名为$pkgname-$pkgver.pkg.tar.gz的新文件。
这个文件可以使用pacman -U 安装一下试试呗~,不过，一个包被构建并不代表你的工作就完成了！
只有当所有文件的结构都正确才能确保完成，例如前缀不对就不行。
可以使用pacman的查询功能显示软件包包含的文件及依赖的文件，然后将它于你认为正确的对比。"pacman -Qlp <package file>" 和"pacman -Qip <package file>" 可以完成这项工作。

如果包看起来是正确的，那汝的工作就完成了。但是如果汝打算发布这个包或PKGBUILD，还是需要确认确认再确认包的依赖关系。

同样要确保安装的软件确实很完美的运行！就算汝释放了一个包括所有必需文件的包，但是由于一些配置选项使它不能很好的工作，这真是让人恼火。

可以用namcap帮助检查软件包的依赖是否正确:

.. code-block :: bash
    
    # 检查PKGBUILD文件
    $ namcap PKGBUILD
    # 检查某个软件包
    $ namcap <package file name>.pkg.tar.xz
    
Namcap会帮汝干这些事:

    1. 检查PKGBUILD文件里的一些常见错误
    2. 用ldd扫描包中所有的ELF文件，自动报告缺失或可去除的依赖。
    3. 启发式搜寻缺失或冗余的依赖。
    
理想的情况是没有输出(真的么?),如果遇到了错误,去ArchWiki上查找对应的解决方案呗~: `<https://wiki.archlinux.org/index.php/Namcap>`_ 

生成源码包
------------------
用下面的命令生成一个源码包:

.. code-block :: bash
    
    makepkg --source
    
这会在当前目录生成一个 :code:`.src.tar.gz` 文件,汝可以在上传到AUR前先分享给其他人帮汝检查一下呗~


到这里汝应该完成了一个软件包了呗~,下次咱会告诉汝怎么把汝创建的软件包提交到AUR上呗~


