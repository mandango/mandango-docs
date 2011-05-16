Indexes
=======

You can map the indexes also in the config classes, and ensure them
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

You can map indexes also in the embedded documents, the full mongo field name
is built automatically::

    array(
        'Model\Comment' => array(
            'isEmbedded' => true,
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
            'embeddedsMany' => array(
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

    $articleRepository->ensureIndexes();
    $authorRepository->ensureIndexes();

All at once through the mandango::

    $mandango->ensureIndexes();

.. _Mongo documentation about indexes: http://www.mongodb.org/display/DOCS/Indexes
