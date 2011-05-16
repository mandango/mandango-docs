Extensions
==========

The extensions allow you to add functionalities to documents, repositories,
queries, in an **extremely flexible** way. It is also very amusing and easy.

Mandango carries by default with some extensions apart from the Core one.

DocumentArrayAccess
-------------------

The ``DocumentArrayAccess`` extension adds the ``\ArrayAccess`` interface to the
documents.

Adding the extension to mondator::

    $extensions[] = new \Mandango\Extension\DocummentArrayAccess();

Using the extension::

    $article = $mandango->create('Model\Article');
    $article['title'] = 'Mandango rocks!';

    echo $article['title']; // Mandango rocks!

DocumentPropertyOverloading
---------------------------

With the ``DocumentPropertyOverloading`` extension you will be able to work with your
documents by **overload**.

Adding the extension to mondator::

    $extensions[] = new \Mandango\Extension\DocummentPropertyOverloading();

Using the extension::

    $article = $mandango->create('Model\Article');
    $article->title = 'Mandango rocks!';

    echo $article->title; // Mandango rocks!

Creating extensions
-------------------

The mandango extensions are mondator extensions, so to create extensions you
must know how to create mondator extensions. That's why I recommend you to
read the :doc:`Mondator documentation </mondator/index>` if you haven't done it
yet.
