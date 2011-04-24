Basic Mapping
=============

The mapping is done with the config classes.

Connection
----------

You can define explicitly the connection in the documents. Otherwise the
default connection of the mandango will be used::

    array(
        'Model\Article' => array(
            // defining explicitly the connection
            'connection' => 'global',
        ),
        'Model\Author' => array(
            // another explicitly connection
            'connection' => 'local',
        ),
        'Model\Category' => array(
            // nothing, default connection
        ),
    );

.. note:
  To specify the connections it is used the connection names that
  are assigned to the Mandango.

Collection
----------

You can also define explicitly the collection where the documents are saved in,
and if you don't, the `CamelCase`_ name of the documents class will be used for
that::

    array(
        'Model\Article' => array(
            // defining explicitly the connection
            'collection' => 'articles',
            'fields' => array(
                'title' => 'string',
            ),
        ),
    );

Types
-----

Mandango has types to allow map data between PHP and Mongo.

* **bin_data**
* **boolean**
* **date**
* **float**
* **integer**
* **raw**
* **serialized**
* **string**

You can also define custom types. We'll see how later.

Fields
------

The fields are the data of the documents, the same as in Mongo.

To define the fields you have to indicate the type::

    array(
        'Model\Article' => array(
            'fields' => array(
                // as string
                'title' => 'string',
                // as array
                'content' => array('type' => 'string'),
            ),
        ),
    );

You can define also options in the fields.

Default Value
^^^^^^^^^^^^^

If you want you can use default values::

    array(
        'Model\Article' => array(
            'fields' => array(
                'is_active' => array('type' => 'boolean', 'default' => true),
            ),
        ),
    );

Database Field
^^^^^^^^^^^^^^

You can change the field in which the data is saved in the database. Thus you
can use a different field name in PHP and Mongo::

    array(
        'Model\Article' => array(
            'fields' => array(
                'php_field_name' => array('type' => 'string', 'db_field' => 'mongo_field_name'),
            ),
        ),
    );

These are the options the Mandango's Core extension accepts, but you can use
more options depending on the extensions you use.

.. note::
  You can define as fields as you want to take advantage of the Mongo free
  schema. Mandango has been designed thinking in that, and to define lots
  of fields is not a performance problem.

.. _CamelCase: http://en.wikipedia.org/wiki/CamelCase
