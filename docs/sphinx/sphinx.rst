Read the Docs: 文档简化
======================

1. 添加连接方法
-------------
.. meta::
   :description lang=en: 在阅读文档时，不断自动构建，版本控制和托管您的技术文档。

`添加连接`_   添加连接方法

添加连接内容信息



.. _添加连接: https://www.baidu.com/


.. prompt:: bash $
	make html
	
	`添加连接`_   添加连接方法

	添加连接内容信息


	.. _添加连接: https://www.baidu.com/


2. 插入单行代码和多行代码
---------------------

插入单行代码方法如下:
	在需要插入单行代码 头尾加上``即可``

插入多上代码方法如下:
	在需要插入多行代码前插入:  .. prompt:: bash $

Assuming you have Python already, `install Sphinx`_:

.. prompt:: bash $

    pip install sphinx
    cd /path/to/project
    mkdir docs


3. 其他文件
----------

  你是软件文档的新手

  或者您是否希望将现有文档与阅读文档一起使用？

  了解文档创作工具，如Sphinx和MkDocs

  帮助您为项目创建出色的文档。


4. 插入表格
----------


+---------------+---------------------+
| STYLE         | BADGE               |
+===============+=====================+
| flat          | |Flat Badge|        |
+---------------+---------------------+
| flat-square   | |Flat-Square Badge| |
+---------------+---------------------+
| for-the-badge | |Badge|             |
+---------------+---------------------+
| plastic       | |Plastic Badge|     |
+---------------+---------------------+
| social        | |Social Badge|      |
+---------------+---------------------+



5. 插入Note
-----------

.. note::
    We in fact are parsing your tag names against the rules given by
    `PEP 440`_. This spec allows "normal" version numbers like ``1.4.2`` as
    well as pre-releases. An alpha version or a release candidate are examples
    of pre-releases and they look like this: ``2.0a1``.

    We only consider non pre-releases for the ``stable`` version of your
    documentation.

    