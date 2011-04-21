Introduction
============

Mandango is an easy, poweful and ultrafast *Object Document Mapper* (ODM) for
PHP and MongoDB.

¿What is MongoDB?
-----------------

MongoDB (*Mongo*) is a scalable, high-performance, open source,
document-oriented database.

Mongo (and the NOSQL_ movement in general) answer to the
**needs for performance** of databases, that each time more applications
(especially web) have, due to the high number of reads, writes and the high
amount of information to store.

.. note::
  If you want to know more about MongoDB, you can visit its web: `http://www.mongodb.org`_

¿What is an ODM?
-----------------

An ODM is a tool that maps the structure of the databases in objects
(PHP in this case). This way you can work in your documents from the database
with PHP objects, with all the great advantages that it carries.

An ODM also abstracts usual operations with the documents, and provides with
utilities that can simplify a lot common tasks.

An ODM also has a disadvantage: the performance. This is due to using
objects and abstracting operations.

¿Why an ODM with MongoDB?
--------------------------

Apparently the Mongo philosophy doesn't agree with the philosophy of ODM,
because on one hand Mongo speeds the database and on the other hand the ODM
slow down the applications. It's true, but we don't have to forget of the
rapid application development.

The *Rapid Application Development* **RAD** is very important in order that
your applications are running as soon as it is possible, because it isn't
worthwhile to spend a lot of time finishing a superfast application if when it
is going to be in production it can't compete or it's unmaintainable.

¿Why Mandango?
------------------

Mandango is to the ODMs what Mongo is to databases.

* **Simple**: Mandango is developed in a simple way. This makes it very easy to learn, use, and avoid bugs.
* **Powerful**: Mandango is very flexible thanks to Mondator, so you'll be able to use it to develop any type of application.
* **Ultrafast**: Mandango has been designed to be extremely light in memory consumption and processing cost.

¿Aren't you convinced yet? Let me show you a few more features:

* **References and Embeds**: Mandango allows you to work with `references and embeds`_ very easily.
* **Extensions**: Mandango can be customized infinitely with Mondator Extensions.
* **Indexes**: Mandango allows you to work easily with the indexes_ of the collections.
* **Events**: Mandango throws _hooks_ before and after inserting, updating, saving and deleting documents.
* **GridFS**: Mandango allows to save files of any size using GridFS_.
* **Log**: Mandango allows to save logs of the queries to improve the development.
* **batchInsert**: Mandango uses batchInsert_ to insert documents in an efficient way.
* **Atomic Operations**: Mandango uses `atomic operations`_ to update and delete documents efficiently.
* **Integratión with IDEs**: Mandango uses generated code, so you may integrate it with your IDE.
* **Tested**: Mandango is completely tested with automated test with PHPUnit_.

.. note::

	 Mandango is **the fastest mapper** in PHP by far.
	 More information in the `performance comparison`_.

Requirements
------------

From your computer
^^^^^^^^^^^^^^^^^^

Mandango requires PHP 5.3.0 or greater, and of course MongoDB and the PHP
driver for MongoDB.

From you
^^^^^^^^

Mandango uses `object oriented PHP`_ and Mongo_, so if you want to start using it
you should know both things minimally at least.

Instalation
-----------

You can install Mandango downloading it directly, by git, svn.

More information in the `Mandango installation page`_.


.. _NOSQL: http://en.wikipedia.org/wiki/NoSQL
.. _performance comparison: http://mandango.org/performance
.. _http://www.mongodb.org: http://www.mongodb.org
.. _references and embeds: http://www.mongodb.org/display/DOCS/Schema+Design#SchemaDesign-Embedvs.Reference
.. _indexes: http://www.mongodb.org/display/DOCS/Indexes
.. _GridFS: http://www.mongodb.org/display/DOCS/GridFS
.. _batchInsert: http://www.php.net/manual/en/mongocollection.batchinsert.php
.. _atomic operations: http://www.mongodb.org/display/DOCS/Atomic+Operations
.. _PHPUnit: http://www.phpunit.de
.. _object oriented PHP: http://www.php.net/manual/en/language.oop5.php
.. _Mongo: http://www.mongodb.org/display/DOCS/Tutorial
.. _Mandango installation page: http://mandango.org/installation
