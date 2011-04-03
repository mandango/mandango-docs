Indexes
=======

You can map the indexes also in the config classes, and synchronize them
easily through the repositories.

Mapping
-------

::

    array(
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
                'slug'    => 'string',
                'date'    => 'date',
            ),
            'indexes' => array(
                // simple
                array('keys' => array('date' => 1)),
                // with options
                array('keys' => array('slug' => 1), 'options' => array('unique' => 1)),
            ),
        ),
    );

You can map indexes even in the embedded documents, the full mongo field name
is built automatically::

    array(
        'Model\Comment' => array(
            'is_embedded' => true,
            'fields' => array(
                'name' => 'string',
                'text' => 'string',
                'date' => 'date',
            ),
            'indexes' => array(
                array('keys' => array('date' => 1)),
            ),
        ),
        'Model\Article' => array(
            // ...
            'embeddeds_many' => array(
                'comments' => array('class' => 'Model\Comment'),
                // index: comments.date
            ),
        ),
    );

.. note::
  You can see the `Mongo documentation about indexes`_ for further information.

Synchronizing
-------------

::

    \Model\Article::repository()->ensureIndexes();
    \Model\Author::repository()->ensureIndexes();

All at once through the mandango::

    $mandango->ensureIndexes();

.. _Mongo documentation about indexes: http://www.mongodb.org/display/DOCS/Indexes
