CakePHP 一览
###################

CakePHP旨在使普通的Web开发任务变得简单而容易。 通过给你提供一个全功能的工具箱来启动CakePHP的各个部分，
它们可以很好的结合在一起或者单独的工作。

本文介绍的是CakePHP中的一些常用概念，让你快速的了解一下这些概念在CakePHP中是如何实现的，
如果你渴望开始一个项目，你可以从这个 :doc:`教程</tutorials-and-examples/cms/installation>` 
开始，或者深入到 :doc:`文档</topics>`。



约定由于配置
==============================

CakePHP 提供了一个基本的组织结构，涵盖了类名，文件名，数据库表明以及其它的约定；尽管这些约定需要花费
一些时间来学习，但按照CakePHP 提供的约定可以避免不必要的配置以及可以创建一个统一的应用程序结构从而使
得各种项目的工作变得简单，查看 :doc:`约定章节</intro/conventions>` ，该章节介绍了 CakePHP 使用的各种
约定。


Model 层
===============

Model 层在应用程序中代表业务逻辑实现的部分(译者注：即MBVC中的B,在Cake中B与M是合并的)。它主要负责检索数据并且
把它转换成应用中需要的数据形式。也用来做数据处理、验证、关联以及其它的与数据处理有关的任务。

在社交网络应用的例子中，Model 层将负责存储用户数据，保存与好友的关联关系，存储和检索用户的照片，推荐新的好友等等。
Model 对象假设为 Friend", "User", "Comment" 或者 "Photo",如果我们打算从  ``users`` 表取出一些数据，我们可以
这么做::

    use Cake\ORM\TableRegistry;

    $users = TableRegistry::get('Users');
    $query = $users->find();
    foreach ($query as $row) {
        echo $row->username;
    }

你可能注意到在处理数据之前我们并没有写任何代码。通过使用约定， CakePHP 会对那些没有定义的table和entity使用标准类(stdClass) 

如果我们打算创建一个新用户并且保存它（带验证），我们可以这么做::

    use Cake\ORM\TableRegistry;

    $users = TableRegistry::get('Users');
    $user = $users->newEntity(['email' => 'mark@example.com']);
    $users->save($user);

View 层
==============

View 层是用来渲染模型数据的。它可以使用 Model 提供的数据生成应用程序可能需要的任一表现形式。

例如，View可以使用模型数据来渲染包含该模型的 html 视图模板，或者使用XML格式化以供其它使用::

    // In a view template file, we'll render an 'element' for each user.
    <?php foreach ($users as $user): ?>
        <li class="user">
            <?= $this->element('user_info', ['user' => $user]) ?>
        </li>
    <?php endforeach; ?>

视图层提供了一些列的扩展像 :ref:`view-templates`, :ref:`view-elements` 和 :doc:`/views/cells`
来让你复用你的表现逻辑。

View 层不仅仅局限于数据的 html 或 text 的表现形式。它可以用来提供一些常见的数据格式，比如 JSON，XML, 
通过它的可插拔架构，你可以轻易扩展实现一些其它的你可能需要的格式，比如CSV

Controller 层
====================

Controller 层用来处理用户的请求(request)，它负责借助Model和View层来渲染一个响应(response)。

一个控制器可以被看作是一个管理者，确保完成任务所需的所有资源都被委派给正确的工作人员。它等待来自客户端的请求，
根据认证或授权规则检查其有效性，将数据获取或处理委托给模型，选择客户端正在接受的表示数据的类型，最后将渲染过程
委派给视图层。 下面是用户注册控制器的例子::

    public function add()
    {
        $user = $this->Users->newEntity();
        if ($this->request->is('post')) {
            $user = $this->Users->patchEntity($user, $this->request->getData());
            if ($this->Users->save($user, ['validate' => 'registration'])) {
                $this->Flash->success(__('You are now registered.'));
            } else {
                $this->Flash->error(__('There were some problems.'));
            }
        }
        $this->set('user', $user);
    }

你可能注意到我们没有明确渲染一个视图，CakePHP的约定将负责选择正确的视图，并使用我们用 ``set()``
设置的视图数据来渲染它。

.. _request-cycle:

CakePHP 请求周期
=====================

现在你可能以及熟悉了CakePHP中的不同层级，现在让我们复习下CakePHP中的一个完整的请求周期:


.. figure:: /_static/img/typical-cake-request.png
   :align: center
   :alt: 一个典型的CakePHP请求的流程图

典型的CakePHP请求流程从用户请求应用程序中的页面或资源开始。 在高层次上，每个请求都经过以下步骤：

#. Web服务器的重写规则将请求定向到 **webroot/index.php** 脚本上。
#. 你的应用程序被加载并绑定到一个 ``HttpServer``。
#. 你的应用的中间件(Middleware)被初始化。
#. request和response被分发到你的应用使用的 PSR-7 中间件上，通常包括错误捕获和路由。
#. 如果中间件没有返回 response，并且 request 中包含路由相关的信息，那么控制器和动作(controller & action)将会被选择。
#. Controller 的 action 被调用，控制器将会和所需的模型(Model)和组件(Component)进行交互
#. Controller 将 response 的创建委托给 View 以生成模型数据产生的输出。
#. View 使用 Helper 和 Cells 来生成 response 的 body 和 header 部分。
#. response 通过 :doc:`/controllers/middleware` 被返回。
#.  ``HttpServer`` 向web服务器发出响应。

这只是个开始
==============

希望这个快速的概述激起了你的兴趣。 CakePHP中的其他的一些强大的功能：

* 集成了 Memcached, Redis,和其它后端的 :doc:`缓存 </core-libraries/caching>` 框架。
* 强大的 :doc:`代码生成工具 </bake/usage>` 可以让你快速开始。
* :doc:`集成测试框架 </development/testing>` 来确保你的代码可以正确的工作。

下一步 :doc:`下载 CakePHP </installation>`, 阅读 :doc:`教程 </tutorials-and-examples/cms/installation>`.


附加阅读
==================

.. toctree::
    :maxdepth: 1

    /intro/where-to-get-help
    /intro/conventions
    /intro/cakephp-folder-structure

.. meta::
    :title lang=en: Getting Started
    :keywords lang=en: folder structure,table names,initial request,database table,organizational structure,rst,filenames,conventions,mvc,web page,sit
