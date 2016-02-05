AUR 纯萌新向入门教学(3)-提交软件包到AUR
===================================================

:slug: aur_sumbiting_guidebook
:lang: zh
:date: 2016-02-05
:tags: Arch Linux,AUR,Guides
:series: Arch Linux

.. PELICAN_BEGIN_SUMMARY

通过上一次的 `「创建一个软件包」 </aur_packaging_guidebook.html>`_ ,
汝应该已经创建了一个 :del:`(或是 N 个)` 软件包了吧,如果汝想分享给其它人的话,上传到 AUR 其实是最方便的方法呗~

.. PELICAN_END_SUMMARY

.. alert-info ::
    
    前几天被 :irc:`archlinux-cn` 的各位吐槽了中文和英文之间空格的问题,原谅咱写文章时太随性😂
        
.. contents::

再来回顾一下 Arch User Repository 的打包规范呗~
-----------------------------------------------------------------

:del:`不合规范的软件包可能会在不经过提醒的话直接删除.`

* `Arch Linux 打包标准 <https://wiki.archlinux.org/index.php/Arch_packaging_standards_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)>`_
    
    对于某些特定平台的软件包(例如 Web 应用)有不同的打包规范,记得看哦~
    
* `提交软件包到 AUR 的规则 <https://wiki.archlinux.org/index.php/Arch_User_Repository_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#.E6.8F.90.E4.BA.A4.E8.BD.AF.E4.BB.B6.E5.8C.85.E7.9A.84.E8.A7.84.E5.88.99>`_

......

看了 Wiki 以后,是否觉得自己的软件包符合规范了?
 
如果确定的话,接着往下看呗~
  
第一步:注册一个 AUR 帐号
-----------------------------------------------------------------

去 `<https://aur.archlinux.org/register/>`_ 注册一个帐号呗~

第二步:为 AUR 准备一个 SSH 密钥
-----------------------------------------------------------------

因为 AUR 现在用 Git 提交,所以没有一个 SSH 密钥是不行的呐~,
建议为 AUR 生成一个新的证书,这样一旦发现问题就可以直接吊销诶(不要把鸡蛋放在一个篮子里~)

可以用 :code:`ssh-keygen` 命令生成新的密钥:

.. code-block:: bash

    $ ssh-keygen 
    Generating public/private rsa key pair.
    # 为汝的公钥和私钥决定一个存放的位置呗~
    Enter file in which to save the key (/home/horo/.ssh/id_rsa):
    /home/horo/.ssh/example
    # 为私钥设置一个密码,可以省略,但是为了安全还是设置一个呐~
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    # 汝的私钥保存在汝决定的路径中
    Your identification has been saved in /home/horo/.ssh/example.
    # 汝的公钥保存在汝决定的路径中,不过扩展名为.pub
    Your public key has been saved in /home/horo/.ssh/example.pub.
    # 这是汝的密钥指纹,用来区分不同的密钥
    The key fingerprint is:
    SHA256:mwk7FvA2E0ycw+E8NYOr1+OL3+0qnF6PFMZ/Ndxuw84 horo@Yotisu
    The key's randomart image is:
    +---[RSA 2048]----+
    |     oo++        |
    |     =*. o       |
    |    . *o         |
    |     o.o .    . .|
    |     .B.S +    oo|
    |    ...*o= o  .o.|
    |     .+o+oo . .oo|
    |     . o=+ + .o..|
    |      .o+o+o+  E |
    +----[SHA256]-----+
    
然后在 AUR Web 界面上点击 "My Account (我的账户)" ,把汝的公钥里的内容填进 "SSH Public Key:" 一节中,保存.

接下来编辑 :code:`~/.ssh/config` ,
告诉ssh命令连接到 :code:`aur.archlinux.org` 用汝新创建的密钥呗~

.. code-block:: bash
    
    Host aur.archlinux.org
        IdentityFile ~/.ssh/example # 记得用汝自己的私钥路径 
        User foo # 记得换成汝自己的用户名.
        
第三步:提交软件包到 AUR 
-------------------------------------------

用下面的命令创建一个新的仓库:

.. code-block:: bash

    # 用汝希望的名称替换foobar.
    # 从不存在的仓库中克隆或推送，将会自动创建此仓库。 
    git clone ssh://aur@aur.archlinux.org/foobar.git
    
这时汝的当前目录下会多出一个以汝的软件包名命名的文件夹(例如 :code:`foobar` ),
把汝的软件包需要的文件(PKGBUILD,有时还有些其他的文件)放到这个文件夹内.

接着记得写一个 :code:`.SRCINFO` (供 AUR Web 界面解析的元数据),
可以通过 :package:`pkgbuild-introspection` 包内的 :code:`mksrcinfo` 工具生成.

.. alert-warning::

    每一次提交都要包含最新的 :code:`.SRCINFO` 文件!不然服务器会 :del:`傲娇的` 拒绝汝的提交呐~
    
然后普通的使用 Git 来提交呗~

.. code-block:: bash
    
    # 还是老话,不要照抄,按汝实际的状况来.
    git add .
    git commit -m "Example"
    git push
    
在 AUR 上搜索汝的软件包试试?(像这样 :aur:`parsoid-git`)

可能的后续工作
------------------------

汝以为把软件包提交上就结束了?

* 一旦上游更新了,汝就要及时的更新诶(年久失修的包会被删除)

* 关注下方的评论,聆听用户的 :ruby:`建议|抱怨` 并试着改进汝的软件包呗~

* 发觉自己没有精力维护某个软件包?可以通过AURweb界面 disown 一个软件包或是在AUR邮件列表发条消息.


