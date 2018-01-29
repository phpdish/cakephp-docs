CMS 教程 - 创建数据库
####################################

现在我们已经安装了 CakePHP, 接下来让给 我们的 :abbr:`CMS(内容管理系统)` 应用创建数据库。
先创建一个空的数据库，命名为``cake_cms``。 执行下面的 SQL 语句创建必要的表::

    USE cake_cms;

    CREATE TABLE users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        email VARCHAR(255) NOT NULL,
        password VARCHAR(255) NOT NULL,
        created DATETIME,
        modified DATETIME
    );

    CREATE TABLE articles (
        id INT AUTO_INCREMENT PRIMARY KEY,
        user_id INT NOT NULL,
        title VARCHAR(255) NOT NULL,
        slug VARCHAR(191) NOT NULL,
        body TEXT,
        published BOOLEAN DEFAULT FALSE,
        created DATETIME,
        modified DATETIME,
        UNIQUE KEY (slug),
        FOREIGN KEY user_key (user_id) REFERENCES users(id)
    ) CHARSET=utf8mb4;

    CREATE TABLE tags (
        id INT AUTO_INCREMENT PRIMARY KEY,
        title VARCHAR(191),
        created DATETIME,
        modified DATETIME,
        UNIQUE KEY (title)
    ) CHARSET=utf8mb4;

    CREATE TABLE articles_tags (
        article_id INT NOT NULL,
        tag_id INT NOT NULL,
        PRIMARY KEY (article_id, tag_id),
        FOREIGN KEY tag_key(tag_id) REFERENCES tags(id),
        FOREIGN KEY article_key(article_id) REFERENCES articles(id)
    );

    INSERT INTO users (email, password, created, modified)
    VALUES
    ('cakephp@example.com', 'sekret', NOW(), NOW());

    INSERT INTO articles (user_id, title, slug, body, published, created, modified)
    VALUES
    (1, 'First Post', 'first-post', 'This is the first post.', 1, now(), now());

你可能已经注意到了 ``articles_tags`` 表使用了复合主键，CakePHP 允许你使用复合主键让你的
表结构保持简单而不必添加额外的 ``id`` 字段。

我们使用数据表和字段名不是随意的，通过使用 CakePHP 的:doc:`命名约定</intro/conventions>`
可以更有效的使用CakePHP，避免去配置框架。虽然 CakePHP 足够的灵活可以适配多种数据库结构，但
为了节省你的时间，你可以使用CakePHP 提供的默认约定。

Database 配置
======================

下一步，我们要告诉 CakePHP 数据库在哪，该怎么连接上它，打开**config/app.php** 文件，
替换 `Datasources.default``数组。下面是一个完整的配置信息数组::

    <?php
    return [
        // More configuration above.
        'Datasources' => [
            'default' => [
                'className' => 'Cake\Database\Connection',
                'driver' => 'Cake\Database\Driver\Mysql',
                'persistent' => false,
                'host' => 'localhost',
                'username' => 'cakephp',
                'password' => 'AngelF00dC4k3~',
                'database' => 'cake_cms',
                'encoding' => 'utf8mb4',
                'timezone' => 'UTC',
                'cacheMetadata' => true,
            ],
        ],
        // More configuration below.
    ];

一旦你保存了  **config/app.php**  文件，刷新默认的欢迎界面，你会发现 'CakePHP is
able to connect to the database' 节也出现了绿色的厨师帽.

.. note::
   
    **config/app.default.php** 文件是配置文件的副本。

生成第一个模型
========================

模型是 CakePHP 应用的核心。它可以让我们查询和修改我们的数据，创建数据的
关联关系，以及验证数据。模型是我们创建控制器行为和模板文件的基础。

CakePHP 的模型是由 ``Table`` 对象和 ``Entity``对象的组成的。``Table`` 对象提供了
对存储在特定表的数据集访问方式，它们存储在 **src/Model/Table**。我们将要创建的会存储为
**src/Model/Table/ArticlesTable.php**，完整的文件像这个样子::

    <?php
    // src/Model/Table/ArticlesTable.php
    namespace App\Model\Table;

    use Cake\ORM\Table;

    class ArticlesTable extends Table
    {
        public function initialize(array $config)
        {
            $this->addBehavior('Timestamp');
        }
    }

我们添加了一个 :doc:`/orm/behaviors/timestamp` 行为，这会让模型自动地处理表中的``created`` 和 
``modified`` 字段。通过把 Table 对象命名为 ``ArticlesTable``，根据命名约定 CakePHP 会知道这个模型
使用的是 ``articles`` 表，并且主键是 ``id`` 字段。

.. note::
    
    如果在 **src/Model/Table** 位置没有找到正确的文件， CakePHP 会为你动态的创建一个模型对象。这也意味着
    如果你不小心把文件名弄错了（比如 articlestable.php 或者 ArticleTable.php）， CakePHP 将不会识别你的
    任何设置，而使用动态生成的模型。


我们也要为我们的 Articles 创建一个 Entity 类，实体代表数据库中的一行记录，给我们的数据提供行级别的行为扩展。
我们的实体会被保存在 **src/Model/Entity/Article.php**，完整的文件类似下面的样子::

    <?php
    // src/Model/Entity/Article.php
    namespace App\Model\Entity;

    use Cake\ORM\Entity;

    class Article extends Entity
    {
        protected $_accessible = [
            '*' => true,
            'id' => false,
            'slug' => false,
        ];
    }

我们的 Entity 实现的非常苗条，我们仅需要设置 ``_accessible`` 属性即可，这个控制那些属性可以被修改:ref:`entities-mass-assignment`
。

我们的模型做得差不多了，下一步我们来创建第一个 :doc:`控制器和模板 </tutorials-and-examples/cms/articles-controller>`来集成我们的
模型。
