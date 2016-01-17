新约伊兹的萌狼乡手札诞生全过程伪实录
===========================================

:slug: new_yoitsu_birth_notes
:lang: zh
:date: 2015-01-18 09:35
:tags: yoitsu ,pelican,blog,python
:series: Pelican

.. PELICAN_BEGIN_SUMMARY

说好的下一期来啦~,新的"约伊兹的萌狼乡手札"是怎么样诞生呐~马上就告诉汝呗~

.. PELICAN_END_SUMMARY

.. contents::

安装Pelican然后进行初始设置
----------------------------

在咱写这篇文章时, :fref:`farsserfc` 已经把Pelican打好包放进Arch Linux 官方软件仓库啦好棒~

Arch Linux用户可以这样安装:

.. code-block:: bash

    sudo pacman -S pelican

其它操作系统可以通过pip安装:

.. code-block:: bash

    sudo pip install pelican

接着运行设置程序建立一个工作文件夹:

.. code-block:: bash

    pelican-quickstart

接着开始挖坑呗~,用 `reStructuredText <http://docutils.sourceforge.net/rst.html>`_ 或 `Markdown <http://wowubuntu.com/markdown/>`_ 开始写文章然后放到 :code:`contents` 文件夹里,像这样:

(reStructuredText)

.. code-block:: rst

    这里是标题
    ===========================================

    :lang: 这里填语言
    :date: 写文章的日期
    :Category: 分类

    这里是内容


(Markdown)

.. code-block:: markdown

    Title: 这里是标题
    date: 写文章的日期
    Category: 分类

    这里是内容

然后运行一个命令来测试

.. code-block:: bash

    make html # 生成html
    make serve # 在127.0.0.1:8000 运行一个测试服务器.

然后打开浏览器输入 :code:`localhost:8000` ，你就能看到一个初生的很 :ruby:`简洁|难看` 的博客了，不过不要担心，它是只丑小鸭，很快就会像天鹅般美丽(真的么?)。

:ruby:`修改|调教` Pelican的主题
---------------------------------------

可能是一时抽风没找到合适的Material Design风格的框架，:del:`于是走上了Metro UI CSS的不归路......`

Metro UI CSS的项目主页在这里 `<http://metroui.org.ua>`_

咱拿了pelican内置的simple主题做起步,把Metro UI CSS文件夹里的 :code:`/css` 和 :code:`/js` 复制到主题的 :code:`/statics` 文件夹里. 

现在的文件夹结构大概像这样:

.. raw:: html

    <pre>
    <span style="color:blue;font-weight:bold;">.</span>
    ├── <span style="color:blue;font-weight:bold;">cache</span>             生成頁面的 pickle 緩存
    ├── <span style="color:blue;font-weight:bold;">content</span>           讀取的全部內容
    │   ├── <span style="color:blue;font-weight:bold;">&lt;categories&gt;</span>      按分類存放的文章
    │   ├── <span style="color:blue;font-weight:bold;">pages</span>             像 About 這樣的固定頁面
    │   └── <span style="color:blue;font-weight:bold;">static</span>            文章內用到的靜態內容
    ├── <span style="color:blue;font-weight:bold;">drafts</span>            文章的草稿箱
    ├── <span style="color:green;font-weight:bold;">Makefile</span>          生成用的 makefile
    ├── <span style="color:green;font-weight:bold;">pelicanconf.py</span>    測試時用的快速 Pelican 配置
    ├── <span style="color:green;font-weight:bold;">publishconf.py</span>    部署時用的耗時 Pelican 配置
    ├── <span style="color:teal;font-weight:bold;">output</span>          -&gt; <span style="color:blue;font-weight:bold;">../kenookamihoro.github.io</span>
    ├── <span style="color:teal;font-weight:bold;">plugins</span>         -&gt; <span style="color:blue;font-weight:bold;">../plugins</span>
    └── <span style="color:teal;font-weight:bold;">theme</span>           -&gt; <span style="color:blue;font-weight:bold;">../yoitsu</span>
    </pre>
        
然后这个内容 repo 中的三个符号链接分别指向三个子 repo（为啥没用 :code:`git submodule` ? 因为咱技术不精还不会用......）。 
theme 指向 `yoitsu <https://github.com/KenOokamiHoro/yoitsu>`_ ，是咱修改过的 pelican 主题啦。
plugins 指向 `pelican-plugins <https://github.com/getpelican/pelican-plugins>`_
最后 output 指向 `kenookamihoro.github.io <https://github.com/KenOokamiHoro/kenookamihoro.github.io>`_ 也就是发布的静态网站啦。        


而主题文件夹的结构大概像这样：

.. raw:: html

    <pre>
    <span style="color:blue;font-weight:bold;">.</span>
    ├── <span style="color:blue;font-weight:bold;">static</span>         主题中用到的静态文件，例如js和css            
    ├── <span style="color:blue;font-weight:bold;">templates</span>      供jinja使用的模板页面     
    │   ├── <span style="color:blue;font-weight:bold;">archives.html</span>     文章归档
    │   ├── <span style="color:blue;font-weight:bold;">article.html</span>      每个文章
    │   ├── <span style="color:blue;font-weight:bold;">author.html</span>       作者
    │   ├── <span style="color:blue;font-weight:bold;">base.html</span>         所有模板的基础
    │   ├── <span style="color:blue;font-weight:bold;">category.html</span>     分类
    │   ├── <span style="color:blue;font-weight:bold;">index.html</span>        首页  
    │   ├── <span style="color:blue;font-weight:bold;">page.html</span>         每个页面
    │   ├── <span style="color:blue;font-weight:bold;">pageination.html</span>  分页
    │   ├── <span style="color:blue;font-weight:bold;">search.html</span>       搜索
    │   └── <span style="color:blue;font-weight:bold;">tag.html</span>          标签
    └──
    
    </pre>
    
然后记得修改pelican.conf告诉Pelican那些页面是模板那些页面是直接生成的呐~

.. code-block:: python

    # DIRECT_TEMPLATES 告诉Pelican哪些页面是直接用来生成特定页面的......
    DIRECT_TEMPLATES = (('index', 'archives', 'search'))
    
接下来开始调教主题呗~,直接给出官方的教程呗~ `Pelican doc:Creating Themes <http://docs.getpelican.com/en/3.6.3/themes.html>`_

经过一番 :ruby:`仔细|无脑` 调教以后,就成了汝等现在看到的样子了呐~

PS:咱自己做的这套主题还木有到能拿来复用的程度(原因主要是咱有很多是直接写死在主题里的设置),所以这又是一个坑呗~


装插件
-------------

作为一套博客系统,Pelican自然有很多的插件可以安装呐~,不信的话去看看`pelican-plugins里有多少插件呗~ <https://github.com/getpelican/pelican-plugins>`_

咱启用的插件有这些:

.. code-block:: python

    PLUGINS = ["better_codeblock_line_numbering",
               'tipue_search',
               'neighbors',
               'series',
               "render_math",
               'extract_toc',
               'tag_cloud',
               'sitemap',
               'summary',
               'bootstrapify',
               'twitter_bootstrap_rst_directives']
               
具体的设置流程嘛,请允许咱引用一下 `farseerfc.me:重新設計了 Pelican 的主題與插件 <http://farseerfc.me/redesign-pelican-theme.html#pelican-restructuredtext>`_ 呗~

实现动态格言
-------------------

动态格言的实现来自 :fref:`fiveyellowmice` 啦~(咱不是JavaScript专家呐~,就不谈具体的实现了呗~)

首先是一段修改某一个元素的类的JavaScript(当然还需要 `velocity <http://julian.com/research/velocity/>`_ ):

.. code-block:: html

    <script src="/theme/js/velocity.min.js"></script>
    
    <script>
    document.addEventListener("DOMContentLoaded", function() {
      $(".menu-button").on("touchstart", function() {
        $(".menu-wrapper").removeClass("trigger");
        if ( $(".nav-items").is(":visible") ) {
			$(".nav-items").velocity("finish")
			.velocity("slideUp", { delay: 200, duration: 400, easing: "easeInQuad" })
			.velocity("fadeOut", { delay: 200, duration: 400, easing: "easeInQuad", queue: false });
		} else {
			$(".nav-items").velocity("finish")
			.velocity("slideDown", { delay: 200, duration: 400, easing: "easeOutQuad" })
			.velocity("fadeIn", { delay: 200, duration: 400, easing: "easeOutQuad", queue: false });
		}
      });
    });
   </script>

然后新建一个 Github gist 填上动态格言,像这样(大括号里的内容可以添加多个):

.. code-block:: json

    [
        {
            "content":"example",
            "author":"someone"
        },
    ]
 
 
再写个JavaScript来从JSON中提取格言然后填到html里:

.. code-block:: html
 
    <script>
		document.addEventListener("DOMContentLoaded", function() {
			$(".site-description").after($("<blockquote>").attr("id", "fortune").css("display", "none"));
			$(".site-description").after($("<blockquote>").attr("id", "fortune").css("line-height", "1.4rem"));

			$.getJSON( "https://api.github.com/gists/07ca2edea6e507bf40f5", function(data) {

				fortunes = JSON.parse(data.files["quotes.json"].content);
				randomFortune = fortunes[ Math.floor( Math.random() * fortunes.length ) ];
				if ( randomFortune.author === undefined ) {
					$("#fortune").html( "<p>"+randomFortune.content+"</p>" );
				} else {
					$("#fortune").html( "<p>"+randomFortune.content+"</p>"  + "<small>" + randomFortune.author + "</small>");
				}

				$("#fortune").velocity("slideDown", { duration: 400, easing: "easeOutQuad" });
			});
		});
	</script>
   
最后的效果汝也应该看到了呗~

发布
--------------

通过几条命令可以发布~

.. code-block:: bash
    
    make publish

然后用git提交到Github就好......

好吧这就是咱的全过程啦(雾)




