Timestampable
=============

The *Timestampable* behavior saves automatically the creation and/or update
date in the documents.

Configuration
-------------

::

    array(
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            'behaviors' => array(
                array('class' => 'Mandango\Behavior\Timestampable'),
            ),
        ),
    );

.. hint::
  **Options**:

  * **createdEnabled**: if saving or not the creation date (enabled by default)
  * **createdField**: field used to store the creation date (*created_at* by default)
  * **updatedEnabled**: if saving or not the update date (enabled by default)
  * **updatedField**: field used to store the update date (*updated_at* by default)

Usage
-----

::

    $article = \Model\Article::create()->setTitle('Mandango')->save();

    echo $article->getCreatedAt(); // new \DateTime('now')
    echo $article->getUpdatedAt(); // null

    $article->setContent('Content')->save();

    echo $article->getCreatedAt(); // the same \DateTime()
    echo $article->getUpdatedAt(); // new \DateTime('now')
