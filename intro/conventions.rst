CakePHP 约定
###################

我们是约定优于配置的忠实用户。 尽管需要花点时间学习 CakePHP 的约定，但在以后的工作
中你会节省很多时间。 通过遵循约定，你可以或者免费的功能并且让你从跟踪配置文件的维护
噩梦中解放出来。约定可以让大家有一个统一的开发体验，以方便其它开发者加入进来并提供帮助。


Controller 约定
======================

控制器类名必须是负数，大驼峰形式，并且以 ``Controller`` 结尾。例如 ``UsersController`` 
和 ``ArticleCategoriesController`` 。

控制器里的 ``public`` 方法通常用来作为 "action"，是可以被浏览器访问的。比如 ``/users/view`` 映射
到 控制器``UsersController`` 的 ``view()``方法。``protected`` 或者 ``private`` 方法无法通过
路由访问。

Controller 名称的 URL注意事项
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

正如你所见，单个单词的控制器映射到一个简单的小写形式的url路径上。例如 ``UsersController`` (定义在
文件 **UsersController.php**) 可以从 http://example.com/users. 访问。

虽然你可以用你喜欢的方式路由到多单词的控制器上，但约定是你的Url应该是小写的并且使用“-”连接多单词；可以
使用 ``DashedRoute``轻易实现，因此 ``/article-categories/view-all`` 可以访问到控制器行为
``ArticleCategoriesController::viewAll()`` 上

当你使用``this->Html->link()``创建链接时，你可以使用下面的约定构造Url数组::

    $this->Html->link('link-title', [
        'prefix' => 'MyPrefix' // PascalCased
        'plugin' => 'MyPlugin', // PascalCased
        'controller' => 'ControllerName', // PascalCased
        'action' => 'actionName' // camelBacked
    ]

关于更多的 URL和参数处理，可以参考 route :ref:`routes-configuration`。


.. _file-and-classname-conventions:

文件和类名约定
===============================

通常情况下文件名和类名是相匹配的，遵循 PSR-4 自动加载原则，下面的是正确的例子:

-  控制器类 ``LatestArticlesController`` 对应的文件名 **LatestArticlesController.php**
-  组件类 ``MyHandyComponent`` 对应的文件名 **MyHandyComponent.php**
-  表类 ``OptionValuesTable`` 对应的文件名 **OptionValuesTable.php**.
-  实体类 ``OptionValue`` 对应的文件名 **OptionValue.php**.
-  行为类 ``EspeciallyFunkableBehavior`` 对应的文件名 **EspeciallyFunkableBehavior.php**
-  视图类 ``SuperSimpleView`` 对应的文件名 **SuperSimpleView.php**
-  助手类 ``BestEverHelper`` 对应的文件名 **BestEverHelper.php**

每个文件都位于你的app目录下类的命名空间对应的文件夹下。

.. _model-and-database-conventions:

数据库约定
====================

对应 CakePHP 模型的数据表名是单词的复数并且使用下划线连接的形式。比如 ``users``, ``article_categories``, 
和 ``user_favorite_pages``


有两个单词以上的 字段/列 要使用下划线形式： ``first_name``。

在 hasMany, belongsTo/hasOne 关系中的外键默认被识别为相关表名的单数形式连接上 ``_id``; 所以在 
"Users" hasMany "Articles"关系中; ``articles`` 表通过外键``user_id`` 与 ``users`` 建立参照关系。
如果一个表像 ``article_categories`` 包含多个单词，那么外键应该被命名为 ``article_category_id``。

在被用于模型之间的 BelongsToMany(多对多) 关系的连接表应该以他们将要加入的模型的表命名，否则 bake 命令
将会不工作，按照字母顺序排列（例如是 ``articles_tags`` 而不是 ``tags_articles``），如果你需要在连接表
里加入额外的字段，那么你需要为该表单独创建 entity/table 类。


除了使用自动递增的整数作为主键，你也可以使用 UUID 列， 当你使用``Table::save()`` 保存一条新纪录时，
CakePHP 会自动地创建一个 UUID 值，使用 (:php:meth:`Cake\\Utility\\Text::uuid()`) 生成;


模型约定
=================

``Table``类应该是单词的复数大驼峰形式，并且以 ``Table`` 结尾。比如 ``Table``. ``UsersTable``,
``ArticleCategoriesTable``, 和 ``UserFavoritePagesTable`` 分别匹配数据表 ``users``, ``article_categories``
和 ``user_favorite_pages``。

实体类(Entity) 是单词的单数大驼峰形式，不需要后缀。例如 User``,``ArticleCategory``, 和 ``UserFavoritePage``
分别匹配数据表  ``users``, ``article_categories`` 和 ``user_favorite_pages``。

视图约定
================

视图模板文件要以它对应的控制器的方法名进行命名，使用小写下划线形式。比如 控制器 ``ArticlesController`` 的行为
``viewAll`` 对应的模板位于 **src/Template/Articles/view_all.ctp**。


基本形式是
**src/Template/Controller/underscored_function_name.ctp**.

.. note::

    默认情况下 CakePHP 使用英语映射(inflections)。如果你的数据表/列 使用其它语言命令，你需要增加映射规则(从单数到复数，反之亦然)
    你可以使用 :php:class:`Cake\\Utility\\Inflector` 定义你的映射股则。 查看更多关于:doc:`/core-libraries/inflector`的文档

总结
==========

By naming the pieces of your application using CakePHP conventions, you gain
functionality without the hassle and maintenance tethers of configuration.
Here's a final example that ties the conventions together:

通过使用 CakePHP 的约定来命名应用程序的各个部分，可以避免进行配置和维护的麻烦，下面是最后一个将约定联系在一起的例子。

-  数据表: "articles"
-  Table 类: ``ArticlesTable``, 文件位于 **src/Model/Table/ArticlesTable.php**
-  Entity 类: ``Article``, 文件位于 **src/Model/Entity/Article.php**
-  控制器 class: ``ArticlesController``, 文件位于 **src/Controller/ArticlesController.php**
-  视图模板 **src/Template/Articles/index.ctp**

通过这些约定，CakePHP 知道一个请求 http://example.com/articles/ 会映射到到控制器``ArticlesController``的行为 ``index()``
上，在这个控制器里模型 ``Articles`` 会被自动加载(自动绑定到数据库中的 "articles" 表上)，并且自动渲染一个视图文件。在这个过程中
除了创建必要的类和文件外，没有一点是需要通过使用配置的。

现在你已经了解到了CakePHP的基础知识，你可以尝试通过 :doc:`/tutorials-and-examples/cms/installation 来查看他们是怎么融合在一起的。

.. meta::
    :title lang=en: CakePHP Conventions
    :keywords lang=en: web development experience,maintenance nightmare,index method,legacy systems,method names,php class,uniform system,config files,tenets,articles,conventions,conventional controller,best practices,maps,visibility,news articles,functionality,logic,cakephp,developers
