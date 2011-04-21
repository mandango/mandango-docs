Mapping
=======

The mapping is done with the config classes of Mondator.

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

* **array** (serialized arrays)
* **bin_data** (for files up to 4Mb)
* **boolean** (booleans)
* **date** (dates)
* **float** (floating numbers)
* **integer** (integers)
* **raw** (without processing)
* **string** (strings)

You can also define custom types. We'll see how later.

Fields
------

The fields are the data of the documents, the same as in Mongo.

To define the fields you have to indicate the type and, if you want, more
options. Mandango allows the option *default* to assign a value by default::

    array(
        'Model\Article' => array(
            'fields' => array(
                // as string
                'title' => 'string',
                // as array
                'content' => array('type' => 'string'),
                // as array providing more options
                'is_active' => array('type' => 'boolean', 'default' => false),
            ),
        ),
    );

.. note::
  Mongo allows a *free schema*, but for mapping documents with Mandango
  you must specify the fields you'll use. You can define as much fields as you want,
  Mandango has been designed to take advantage of the free schema of Mongo, so
  to define a lot of fields is not a performance problem.

References
----------

References are relations to other documents. MongoDB doesn't
allow joins or foreign keys, nut you can save references to other documents
and later consult them. Mandango allows you to do this very easily.

There are two types of references, which vary in the number of documents
that are going to be referenced.

* **one**
* **many**

You just have to indicate name of the reference the document class you want
to reference.

The field where the reference will be saved is the same that the name of the
reference.

Let's see some examples::

    array(
        'Model\Author' => array(
            'fields' => array(
                'name' => 'string',
            ),
        ),
        'Model\Category' => array(
            'fields' => array(
                'name' => 'string',
            ),
        ),
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            // to one
            'references_one' => array(
                'author' => array('class' => 'Model\Author'),
            ),
            'references_many' => array(
                'categories' => array('class' => 'Model\Category'),
            ),
        ),
    );

.. note::
  You must indicate the full class.

Embeddeds
---------

The *embedded documents* are documents inside other documents.

There are two types of embeddeds, depending on the number of documents that
they are going to embed.

* **one**
* **many**

To map embeddeds you have to indicate the embedded name and the document class
you are going to embedd.

The embedded documents are mapped in the same way as regular documents, so you
just have to indicate that it's an embedded document activating the option
*is_embedded*::

    array(
        'Model\Comment' => array(
            'is_embedded' => true,
            'fields' => array(
                'name' => 'string',
                'text' => 'string',
            ),
        ),
        'Model\Source' => array(
            'is_embedded' => true,
            'fields' => array(
                'name' => 'string',
                'url'  => 'string',
            ),
        ),
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            // one
            'embeddeds_one' => array(
                'source'   => array('class' => 'Model\Source'),
            ),
            // many
            'embeddeds_many' => array(
                'comments' => array('class' => 'Model\Comment'),
            ),
        ),
    );

.. tip::
  Mandango can work with multiple embedded documents, that is, embed inside the embeddeds.

Relations
---------

The relations are relations **from** other documents. They are the opposite to
references, so when we use relations we have to have been defined the reference
as well.

There are three types of relations, and to use one or the other depends on the
opposite reference.

* **one**: the opposite reference is to one and unique, that is, one to one.
* **many_one**: the opposite reference is to one (but not unique), that is, many to one
* **many_many**: the opposite reference is many, that is, many to many

Let's see some examples::

    array(
        'Model\Author' => array(
            'fields' => array(
                'name' => 'string',
            ),
            // one
            'relations_one' => array(
                'phone_number' => array('class' => 'Model\PhoneNumber', 'reference' => 'author'),
            ),
            // many_one
            'relations_many_one' => array(
                'articles' => array('class' => 'Model\Article', 'reference' => 'author'),
            ),
        ),
        'Model\PhoneNumber' => array(
            'fields' => array(
                'author_id' => 'reference_one',
            ),
            'references_one' => array(
                'author' => array('class' => 'Model\Author'),
            ),
        ),
        'Model\Category' => array(
            'fields' => array(
                'name' => 'string',
            ),
            // many_many
            'relations_many_many' => array(
                'articles' => array('class' => 'Model\Article', 'reference' => 'categories'),
            ),
        ),
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            'references_one' => array(
                'author' => array('class' => 'Model\Author'),
            ),
            'references_many' => array(
                'categories' => array('class' => 'Model\Category'),
            ),
        ),
    );

.. warning::
  The relations cannot be used in embedded documents. If you want to be able
  to reference a document, just use a regular document.

.. _CamelCase: http://en.wikipedia.org/wiki/CamelCase
