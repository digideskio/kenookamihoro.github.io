在 Arch Linux 上安装 Ghost 博客系统
===================================================

:slug: ghost_blog_archlinux
:lang: zh
:date: 2016-02-14
:tags: Arch Linux,partner
:series: Arch Linux

.. PELICAN_BEGIN_SUMMARY

先祝大家 :ruby:`情人节|烧烤节` 快乐~ (2333

:del:`为了让旅伴发发牢骚,就装了一个 Ghost 博客系统咯~`

.. PELICAN_END_SUMMARY

.. contents::

汝连啥是 Ghost 都不知道?
-----------------------------------------------------------------

    Ghost是用JavaScript编写的博客平台，基于MIT许可证开放源代码。Ghost的设计主旨是简化个人网站发布以及网上出版的过程。

    Ghost是一款个人博客系统，它是使用Node.js语言和MySQL数据库开发的，同时支持MySQL、MariaDB、SQLite和PostgreSQL。用户可以在支持Node.js的服务器上使用自己的博客。

    -- `Wikipedia 上的 "Ghost (博客平台)" 条目 <https://zh.wikipedia.org/wiki/Ghost_%28%E5%8D%9A%E5%AE%A2%E5%B9%B3%E5%8F%B0%29>`_
    
简单来说,Ghost 是一套博客平台,是一套博客平台,是一套博客平台! :del:`(重要的事情说三遍😂😂)`

要安装 Ghost 需要啥?
------------------------------------------------------------------

因为 Ghost 是用 Node.js 写成的,所以要安装 Ghost , 汝需要先装上 Node.js 和 npm 呗~

.. alert-warning::
    截至写这篇文章时,Arch Linux 官方源里 Node.js 的版本是 5.6.0 ,而 Ghost 的计划是只支持 Node.js 的 :ruby:`LTS|长期支援` 版本, 而 Node.js 的长期支援版本是 0.10x,0.12x和4.2 .所以嘛...... 
    
    这里(和咱在 AUR 的 ghost 软件包里)用到了一个环境变量 :code:`GHOST_NODE_VERSION_CHECK=false` 来不让 ghost 来检查 node 的版本,在 Node.js 下一个 LTS 版本(6.x)出来前先凑合一下呗~
    

于是先安装 :package:`nodejs` 和 :package:`npm` :

.. code-block:: bash

    sudo pacman -S nodejs npm
    
如果汝认为自己的博客会做的比较大,需要一个数据库系统的话,咱推荐 :package:`mariadb` 呗~

从 AUR 安装 Ghost
--------------------------------------------------------------------
然后从 AUR 安装 :aur:`ghost` (这个包是咱更新的,有问题尽管 pia 咱~).

如果汝有 yaourt 的话, :code:`yaourt -S ghost`

这会把 ghost 安装在 :code:`/srv/ghost/` 目录,由于创建的 ghost 用户不能通过 shell 登录,要修改这个目录的文件的话:

* 修改 :code:`/etc/passwd` 文件:

    ghost:x:738:738::/srv/ghost:/usr/bin/nologin
    
    把 :code:`/usr/bin/nologin` 换成 :code:`/bin/bash` ,保存.
    
    这样以后可以通过 :code:`sudo su ghost` 切换到 ghost 用户对 /srv/ghost 目录写入了.
 
* 通过下面的命令以 ghost 用户运行一条命令:
    
    .. code-block:: bash
        
        # su 后面的 -s 参数可以制定切换用户后运行的 shell , 
        # -c 参数可以指定要运行的命令.
        
        sudo su ghost -s /bin/bash -c "此处是汝的命令,记得带上引号"

通过源代码安装 Ghost 
--------------------------------------------------------------------
首先把 ghost 的源代码下载下来并解开:

.. code-block:: bash 
    
    # 这时最新的版本是0.7.6.
    wget https://ghost.org/zip/ghost-0.7.6.zip
    unzip ghost-0.7.6.zip 
    cd ghost-0.7.6
    
接下来通过 npm 安装需要的依赖,因为上面的提示嘛~

.. code-block:: bash 

    GHOST_NODE_VERSION_CHECK=false npm install
    
修改配置文件
---------------------------------------------------------------------

如果是通过 AUR 安装的,配置文件位于 :code:`/srv/ghost/config.js`

如果是通过源代码安装的,从目录中先复制一份样例出来呗~

    .. code-block:: bash

        cp config.example.js config.js
        

这里的例子是修改 :code:`Production` 一节
(这一节是汝的 Ghost 实际运行时的配置,下面的 Development 一节是开发时的配置)

.. code-block:: javascript

    config = {
        // ### Production
        // When running Ghost in the wild, use the production environment.
        // Configure your URL and mail settings here
        production: {
            // 汝的网址?
            url: 'http://localhost',
            mail: {},
            // 汝想使用那种数据库? 
            // 下面的例子是 sqlite3 数据库,配置文件中还有设置 MariaDB 数据库 的样例
            database: {
                client: 'sqlite3',
                connection: {
                    filename: path.join(__dirname, '/content/data/ghost.db')
                },
                debug: falseProduction
            },
    
            server: {
                host: '127.0.0.1',
                port: '2368'
            }
        },

如果汝的 Ghost 和汝进行操作的电脑是同一个
------------------------------------------------

通过下面的命令来测试汝的 Ghost 呗~

.. code-block:: bash

    cd /path/to/ghost
    GHOST_NODE_VERSION_CHECK=false
    
如果汝是从 AUR 安装的,可以通过 Systemd 来启动

.. code-block:: bash

    sudo systemctl start ghost
    
现在打开 :code:`http://localhost:2368` 看看效果呗~

如果汝的 Ghost 和汝进行操作的电脑不是同一个
-------------------------------------------------------

比如汝在 VPS 上安装了 Ghost,就需要用一个 web服务器通过反向代理来访问呗~

首先修改 :code:`config.js` 把 Production 中的 URL 换成汝的网址啦~

如果汝在用 Nginx, 把这一段增加到汝的 :code:`server` 块中:

.. code-block:: text

     location / {
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header Host $http_host;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_pass http://127.0.0.1:2368;
           # 汝的更多自定义设置
     }
     
如果汝在用 Apache, 把下面一段添加到汝的 httpd.conf 的 vhost 段中(首先要启用 mod_proxy 模块~):

.. code-block:: text

    ProxyPass / http://localhost:2368/
    ProxyPassReverse / http://localhost:2368/
    ProxyHTMLURLMap http://localhost:2368/ /
    RequestHeader set X-Forwarded-For $proxy_add_x_forwarded_for
    RequestHeader set Host $host
    RequestHeader set X-Forwarded-Proto $scheme	


然后重新启动 ghost 和 web 服务器以后试试通过汝的网址访问?

打开 :code:`http://汝的ghost网址/ghost/` 开始设置汝的 Ghost 博客呗~

参考资料
----------------------------------------

* `Ghost Blog 文档 <http://support.ghost.org/developers>`_
* `Apache httpd mod_proxy 文档 <http://httpd.apache.org/docs/2.4/mod/mod_proxy.html>`_
