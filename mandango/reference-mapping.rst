Reference Mapping
=================

References are relations to other documents. MongoDB doesn't
allow joins or foreign keys, nut you can save references to other documents
and later consult them. Mandango allows you to do this very easily.

You just have to indicate name of the reference and the document class you want
to reference.

There are two types of references, which vary in the number of documents
that are going to be referenced.

References One
--------------

::

    array(
        'Model\Author' => array(
            'fields' => array(
                'name' => 'string',
            ),
        ),
        'Model\Article' => array(
            // to one
            'references_one' => array(
                'author' => array('class' => 'Model\Author'),
            ),
        ),
    );

.. note::
  You must indicate the full class.

References Many
---------------

::

    array(
        'Model\Category' => array(
            'fields' => array(
                'name' => 'string',
            ),
        ),
        'Model\Article' => array(
            // to many
            'references_many' => array(
                'categories' => array('class' => 'Model\Category'),
            ),
        ),
    );

