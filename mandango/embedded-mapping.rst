Embedded Mapping
================

The *embedded documents* are documents inside other documents.

To map embeddeds you have to indicate the embedded name and the document class
you are going to embedd.

The embedded documents are mapped in the same way as regular documents, so you
just have to indicate that it's an embedded document activating the option
``isEmbedded``.

There are two types of embeddeds, depending on the number of documents that
they are going to embed.

Embeddeds One
-------------

::

    array(
        'Model\Source' => array(
            'isEmbedded' => true,
            'fields' => array(
                'name' => 'string',
                'url'  => 'string',
            ),
        ),
        'Model\Article' => array(
            // one
            'embeddedsOne' => array(
                'source'   => array('class' => 'Model\Source'),
            ),
        ),
    );

Embeddeds Many
--------------

::

    array(
        'Model\Comment' => array(
            'isEmbedded' => true,
            'fields' => array(
                'name' => 'string',
                'text' => 'string',
            ),
        ),
        'Model\Article' => array(
            // many
            'embeddedsMany' => array(
                'comments' => array('class' => 'Model\Comment'),
            ),
        ),
    );

.. tip::
  Mandango can work with deep embedded documents, that is, embed inside the embeddeds.
