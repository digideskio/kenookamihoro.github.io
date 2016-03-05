在 Windows 上编织 Pelican 博客 -- MSYS2篇
==================================================

:slug: pelican_on_windows_msys2
:lang: zh
:date: 2016-03-04 21:00
:tags: pelican,blog
:series: Pelican 

.. PELICAN_BEGIN_SUMMARY

用 Windows 那是迫不得已……

.. PELICAN_END_SUMMARY

.. contents::

:del:`人生赢家` :fref:`farseerfc` 这样写到......

    寄宿在 Github Pages 上的静态博客通常有两种方案，其一是使用 Jekyll 方式撰写，
    这可以利用 Github Pages 原本就有的 Jekyll支持 生成静态网站。
    另一种是在 本地 也就是自己的电脑上生成好，然后把生成的 HTML 网站 push 到 Github Pages ，
    这种情况下 Github Pages 就完全只是一个静态页面宿主环境。

    我用 Pelican 生成博客，当然就只能选择后一种方式了。
    这带来一些不便，比如本地配置 pelican 还是有一点点复杂的，所以不能随便找台电脑就开始写博客。
    有的时候只是想修正一两个错别字， 这时候必须打开某台特定的电脑才能编辑博客就显得不太方便了。
    再比如 pelican 本身虽然是 python 写的所以跨平台，但是具体到博客的配置方面， Windows 环境和 Linux/OSX/Unix-like 环境下还是有 些许出入 的。
    还有就是没有像 wordpress 那样的基于 web 的编辑环境，在手机上就不能随便写一篇博客发表出来
    （不知道有没有勇士尝试过在 Android 的 SL4A 环境下的 python 中跑 pelican ，还要配合一个 Android 上的 git 客户端 ）。
    
    ---- `Farseerfc.me:用 Travis-CI 生成 Github Pages 博客  <https://farseerfc.me/travis-push-to-github-pages-blog.html>`_
    
确实,Pelican 虽然是跨平台的,但是......

如果汝用了 :code:`pelican-quickstart` ,汝的目录下会有一个 :code:`Makefile` 文件,那么问题来了,
Windows 里上哪读 Makefile 啦(ノ=Д=)ノ┻━┻

所以只好在 Windows 里搞个类 :ruby:`Unix|Linux` 环境了(\´･ω･\`)

:del:`某人:其实把 Makefile 魔改成批处理文件也是可以的$#W#@$##%$^&%^%^$^%&%`

为啥是msys2?
---------------------------------

    :fref:`quininer` :明明是 OneGet + PowerShell 大法好嘛~
    
在 Windows 世界里最出名的类 Unix 环境不是 `Cygwin <http://cygwin.com/>`_ 么?

    因为 msys2 有 pacman 啦~

    因为 msys2 有 pacman 啦 ~ (╯T▽T)╯ ┻━┻

    因为 msys2 有 pacman 啦 ~ (ノ=Д=)ノ┻━┻

    -- :del:`重要的事情说三遍` (๑•̀ㅂ•́)و✧
    
总之为了 pacman 咱最后选了 msys2 😂😂😂

安装和设置 msys2
------------------------------------

去 `官方网站 <http://msys2.github.io/>`_ 或是 `崔主席的镜像源 <http://mirrors.ustc.edu.cn/msys2/Base/>`_ 下载基本组件包啦~ ( :fref:`cuihao` 好棒~ )

如果需要, `把软件仓库换成崔主席的镜像呗~ <https://lug.ustc.edu.cn/wiki/mirrors/help/msys2>`_

接下来更新msys2和安装基本工具 ( 咱用了 Github 所以再装个 Git ):

    pacman -Syu
    pacman -S base-devel make git

.. alert-warning::

    截至写这篇文章时,咱从pacman安装的 pip ( :code:`mingw-w64-x86_64-python3-pip` ) 会因为一个 ImportError 没法装任何软件包呐~ ( pia之 (╯＠ω＠)╯ ┻━┻ )
    
    所以只好装个 Windows 版的 Python 😂
    
安装 pelican 和 Windows 版 Python 
-------------------------------------

`去 Python.org 下载啦~ <https://www.python.org/downloads/windows/>`_ ,记得把 :code:`python` 和 :code:`pip` 添加到系统的 :code:`PATH` 中.( msys2 好像可以用 Windows 的 PATH ~)

接下来打开 msys2 shell (其实就是 Bash 啦 😂) 把 Windows 里的 python 软连接到 :code:`/usr/bin/python` 上

.. code-block:: bash

    # 不知道在哪? 用 whereis 命令查一下啦~
    $ whereis python
    /c/python35/python.exe
    # 用 ln -s <源路径> <目标路径> 创建一个符号链接.
    $ ln -s /c/python35/python.exe /usr/bin/python
    
然后用 pip 安装 pelican _(:з」∠)_

    pip install pelican
    
试验
------------------------------------
先用各种不同的方法把汝的 pelican 文件夹复制到 msys2 的主文件夹里啦~ \
( 在汝安装 msys2 的文件夹中有一个 :code:`home/<汝 Windows 系统的用户名>/` 的文件夹啦  (╯°∧°)╯ ┻━┻ )

如果汝用了 :code:`pelican-quickstart` 生成了 develop_server.sh 那它喂给 sh 啦~

    sh develop_server.sh start
    
如果没有的话,那就自己 make 呗~

.. code-block:: bash

    # 生成html
    make html
    # 运行测试服务器
    make serve
    
有时汝可能用到一些其它程序,那么汝只好通过 pacman 安装或者自己编译啦 (╯‵﹏′)╯ ┻━┻

:del:`这篇文章其实是在 Arch Linux 上完成的所以并没有啥截图😂😂😂`

