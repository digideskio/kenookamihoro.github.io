新的"约伊兹的萌狼乡手札"诞生啦~
=========================================

:slug: new_yoitsu_birth
:lang: zh
:date: 2015-01-16 09:35
:tags: yoitsu ,pelican,blog
:series: Pelican 

.. PELICAN_BEGIN_SUMMARY

就像标题描述的一样，新的"约伊兹的萌狼乡手札"以Pelican之姿再次出发啦~

.. PELICAN_END_SUMMARY

.. contents::

为啥要重做？
----------------------------

原来的 `约伊兹的萌狼乡手札 <https://wiki.yoitsu.moe/>`_ 是基于MediaWiki搭建的,但是MediaWiki的本来用途并不是来做博客的呐~,不过咱还是一直拖着......直到 `Arch Linux 宣布PHP7进入官方软件仓库 <https://www.archlinux.org/news/php-70-packages-released/>`_ ,咱升级以后两个关键的RSS扩展都坏掉啦~(应该都知道RSS对于博客型网站的重要性呗~),于是咱痛定思痛决定升级😂


为啥是Pelican?
---------------------

主要的原因是 :del:`人生赢家` :fref:`farseerfc` 用的也是Pelican，这样咱可以照着他的经历少走一些弯路......

为啥不用Hexo,Ghost一类的博客系统呢?因为咱不太会设置Node.js(想当初给MediaWiki装可视化编辑器就折腾了半天)😂😂😂


那么有哪些问题咧?
---------------------------

首先Pelican用的标记语言是rst(重组的文本)或是MarkDown,不过看情况Markdown是刚加入进来的,处理的还不够好,就先用rst呗~

然而咱并不会rst的语法......只好找来个 `语法指南 <http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html>`_ 先照着看......


关于旧站的打算?
-------------------

介于原来的网站也是咱折腾了一阵子才出来的,于是决定先留着.(这个新站的评论要靠它呐~)

咱以后也会继续折腾MediaWiki,顺便在这记下来一些过程和经验呗~

关于评论?
--------------

目前的计划是用 `<https://wiki.yoitsu.moe/>`_ 的Feedback名字空间或是Github issues来完成呐~汝问咱为啥不用Disqus?

受 :fref:`fiveyellowmice` 的影响,咱不会试图去收集汝的隐私呐~

这个网站回放在咱自己的服务器上,然而由于开了Cloudflare的CDN又没安装插件,咱的服务器日志上的IP地址全部是来自Cloudflare的......

咱还使用Google Analytics ，它收集像访问人数和浏览器类型之类的信息.而且(照Google的说法),它使用匿名标识符来区分每一个人.所以汝最好去看看 `Google 的隐私政策 <https://www.google.com/intl/zh-CN/policies/>`_ .如果汝真的不想让咱知道的话, `用Tor 呗~ <https://www.torproject.org/>`_ 

这里(不含用于评论的wiki.yoitsu.moe)本身不会创建任何Cookie,汝看到的Cookie是CloudFlare和Google Analytics创建的,和咱没啥关系.

用于评论的 `原约伊兹的萌狼乡手札 <https://wiki.yoitsu.moe/>`_ 是基于MediaWiki搭建的,所以匿名用户的IP地址会显示在编辑历史中(这是MediaWiki软件自身的设定,毕竟人家的本意是做开放的wiki不是?).如果汝使用账户登录的话,便会在汝的浏览器上设置Cookies呐~(登录用户的IP地址轻易不会让人知道的啦[#]_ )

当然啦,如果汝觉得便捷大于对隐私的关注的话,可以发个评论抱怨一下咱~


.. [#] 通过Checkuser扩展可以查出注册用户进行编辑时的IP地址,然而咱并没有......


作为新博客的第一篇博文就说到这里,咱具体是怎么做的 :del:`请听下回分解` 😂😂😂
