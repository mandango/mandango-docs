Sluggable
=========

The *Sluggable* behavior saves the slug of one field in another one.

Configuration
-------------

::

    array(
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
            'extensions' => array(
                array('class' => 'Mandango\Behavior\Sluggable', options' => array('from_field' => 'title')),
            ),
        ),
    );

.. hint::
  **Options**:

  * **from_field**: field used to generate the slug (required)
  * **slug_field**: field used to store the slug (*slug* by default)
  * **unique**: if the slugs have to be unique (enabled by default) (if it is enabled a unique index is created)
  * **update**: if the slugs can be updated if the field from where the slug is created is modified (disabled by default)
  * **builder**: function that converts the base string to slug (*Mandango\Behavior\Util\Sluggable::slugify()* by default)

Usage
-----

::

    $article = \Model\Article::create()
        ->setTitle('Mandango is ultrafast!')
        ->save()
    ;

    echo $article->getSlug(); // mandango-is-ultrafast

    $article2 = \Model\Article::create()
        ->setTitle('Mandango is ultrafast!')
        ->save()
    ;

    echo $article2->getSlug(); // mandango-is-ultrafast-2
