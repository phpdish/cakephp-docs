内容管理系统教程
###########################

本教程将引导您创建一个简单的CMS应用程序。首先我们先要安装 CakePHP, 创建我们的数据库，
然后创建简单的文章管理。

下面是你需要做的：

#. 一个数据库系统。我们打算使用 Mysql。为了创建数据库和运行教程里的 SQL 片段，
你需要知道一些关于SQL的知识。CakePHP 会处理创建项目中的所有的查询工作。由于我们
使用的是 Mysql,所以你还需要你的 PHP 中开启了 ``pdo_mysql`` 扩展。

#. 掌握基本的 PHP 知识：

开始之前，你要确保已经安装了一个最新版本PHP：

.. code-block:: bash

    php -v

至少需要 PHP |minphpversion| (CLI) 版本或者更高。

获取 CakePHP
===============

最简单的安装方式是通过 Composer。如果你之前没有安装过 Composer 那么你需要先下载它。
如果你已经安装了 cURl， 那么简单的执行下面命令即可：

.. code-block:: bash

    curl -s https://getcomposer.org/installer | php

或者从 `Composer 官网 <https://getcomposer.org/download/>`_ 下载 ``composer.phar`` 。

Composer 安装完成后，在控制台输入下面命令把 CakePHP 应用骨架安装到工作目录下的 ``CMS``文件夹下：

.. code-block:: bash

    php composer.phar create-project --prefer-dist cakephp/app cms

如果你已经下载并执行了 `Composer Windows 安装程序 <https://getcomposer.org/Composer-Setup.exe>`_,
然后在你的工作目录（假如是C:\\wamp\\www\\dev\\cakephp3）执行下面命令进行安装：

.. code-block:: bash

    composer self-update && composer create-project --prefer-dist cakephp/app cms

使用Composer的最大优势是可以自动地完成一些比较重要的安装步骤，比如设置文件权限，创建配置文件
**config/app.php**。

有很多的方式可以安装 CakePHP。如果你不使用 Composer, 可以参考 :doc:`/installation`  节了解更多。
(译者注：别想着其它方式，你就认为composer是唯一的方式)。

无论你怎么安装 CakePHP, 一旦安装完成，你的安装目录下回出现下面这样的文件结构::

    /cms
      /bin
      /config
      /logs
      /plugins
      /src
      /tests
      /tmp
      /vendor
      /webroot
      .editorconfig
      .gitignore
      .htaccess
      .travis.yml
      composer.json
      index.php
      phpunit.xml.dist
      README.md

现在是个学习一点 CakePHP 目录结构的好时候， 查看这里章节 :doc:`/intro/cakephp-folder-structure`。
如果你在本教程里迷失了，你可以在 `GitHub <https://github.com/cakephp/cms-tutorial>`_ 看到这个项目
最终完成的样子。

检查安装
=========================

我们可以通过打开默认主页来快速的检查一下我们的安装是否正确。在这这钱你需要开启开发服务器:

.. code-block:: bash

    cd /path/to/our/app

    bin/cake server

.. note::

    Windows用户执行  ``bin\cake server``（注意是反斜线，不是斜线）。

上述命令会在开启一个 PHP 内建的服务器监听 8765 端口。在你的浏览器中打开 **http://localhost:8765**  查看
欢迎界面。查看 Environment 和 Filesystem 所有的要求都要满足（能看到绿色的厨师帽即表示检查通过）以及
能够连上数据库。如果不能你就需要安装对应的 PHP 扩展或者设置文件权限。

下一章我们将要 创建:doc:`数据库并且生成第一个模型 </tutorials-and-examples/cms/database>`.
