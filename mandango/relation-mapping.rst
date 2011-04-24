Relation Mapping
================

The relations are relations **from** other documents. They are the opposite to
references, so when we use relations we have to have been defined the reference
as well.

There are three types of relations, and to use one or the other depends on the
opposite reference.

Relations One
-------------

The opposite reference is to one and unique, that is, one to one::

    array(
        'Model\Author' => array(
            'fields' => array(
                'name' => 'string',
            ),
            // one
            'relationsOne' => array(
                'phone_number' => array('class' => 'Model\PhoneNumber', 'reference' => 'author'),
            ),
        ),
        'Model\PhoneNumber' => array(
            'referencesOne' => array(
                'author' => array('class' => 'Model\Author'),
            ),
        ),
    );

Relations Many One
------------------

The opposite reference is to one (but not unique), that is, many to one::

    array(
        'Model\Author' => array(
            'fields' => array(
                'name' => 'string',
            ),
            // many_one
            'relationsManyOne' => array(
                'articles' => array('class' => 'Model\Article', 'reference' => 'author'),
            ),
        ),
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            'referencesOne' => array(
                'author' => array('class' => 'Model\Author'),
            ),
        ),
    );

Relations Many Many
-------------------

The opposite reference is many, that is, many to many::

    array(
        'Model\Category' => array(
            'fields' => array(
                'name' => 'string',
            ),
            // many_many
            'relationsManyMany' => array(
                'articles' => array('class' => 'Model\Article', 'reference' => 'categories'),
            ),
        ),
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            'referencesMany' => array(
                'categories' => array('class' => 'Model\Category'),
            ),
        ),
    );

.. warning::
  The relations cannot be used in embedded documents. If you want to be able
  to reference a document, just use a regular document.
