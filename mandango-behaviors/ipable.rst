Ipable
======

The *Ipable* behavior saves automatically the IP from where the documents are
created and/or updated.

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
                array('class' => 'Mandango\Behavior\Ipable'),
            ),
        ),
    );

.. hint::
  **Options**:

  * **createdEnabled**: if saving or not the IP from where documents are created (enabled by default)
  * **createdField**: field used to store the IP from where documents are created (*created_from* by default)
  * **updatedEnabled**: if saving or not the IP from where documents are updated (enabled by default)
  * **updatedField**: field used to store the IP from where documents are updated (*updated_from* by default)
  * **getIpCallable**: callable that returns the IP to save (*Mandango\Behavior\Util\IpableUtil::getIp()* by default)

Usage
-----

::

    $article = $mandango->create('Model\Article')->setTitle('Mandango')->save();

    echo $article->getCreatedFrom(); // 127.0.0.1
    echo $article->getUpdatedFrom(); // null

    $article->setContent('Content')->save();

    echo $article->getCreatedFrom(); // 127.0.0.1
    echo $article->getUpdatedFrom(); // 127.0.0.1
