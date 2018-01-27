CakePHP 文档结构
########################

当你下载完 CakePHP 的应用骨架之后，你会看到一些顶级的文件夹：


- *bin* 目录保存了 Cake 的命令行程序对应的可执行文件
- *config* 文件夹保存了一些CakePHP使用的 :doc:`/development/configuration` 文件，
  数据库连接信息, 引导文件，一些核心的配置文件以及其它应该被放在这的文件
- *plugins* 目录是存储你的应用使用的 :doc:`/plugins`
- *logs* 目录一般存储你的日志文件， 这决定于你的日志配置
- *src* 目录是放置你应用的源代码的地方。
- *tests* 目录是存放你的应用程序对应的单元测试文件
- *tmp* 目录是 CakePHP 存储的一些临时文件，实际的数据取决于你怎么配置CakePHP, 但是这个目录
  通常存放翻译的信息（编译之后），模型的信息和一些其它的sessions数据。
- *vendor* 目录是用来存放 CakePHP 和被`Composer <http://getcomposer.org>`_安装的应用程序依赖的其它库，
  不推荐修改这个目录下的文件，即使你改了当你下次执行composer 更新的时候也会被覆盖。
- *webroot* 是你的应用程序的跟目录. 它保存了一些可以公开访问资源文件。

要确保 *tmp* 和 *logs* 目录是存在并且可写的，否则你的应用程序性能会被严重影响。在开发模式下
CakePHP 会警告你，如果这两个目录不可写的话。

src 目录
==============

这个目录是你要完成大部分开发工作的地方，让我们仔细看一下这个文件夹*src*。

Controller
    存放你应用程序的控制器和它们所需要的组件
Locale
    存放用于国际化的字符串文件
Model
    存放应用程序的 tables,entities 和behaviors。
Shell
    存放控制台命令和任务；更多的信息请查看:doc:`/console-and-shells`。
View
    渲染层相关的类，包括views, cells, helpers.
Template
    渲染层相关的文件，包括模板碎片化元素(elements), 400/500错误页面(error pages), 布局(layouts), 和模板文件

.. meta::
    :title lang=en: CakePHP Folder Structure
    :keywords lang=en: internal libraries,core configuration,model descriptions,external vendors,connection details,folder structure,party libraries,personal commitment,database connection,internationalization,configuration files,folders,application development,readme,lib,configured,logs,config,third party,cakephp
