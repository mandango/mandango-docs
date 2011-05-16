Working with documents
======================

You have to create the documents through the ``create`` method of the mandango,
passing it the document class::

    $article = $mandango->create('Model\Article');

Mandango uses setters and getters to modify and access to the document's data.

Identifier
----------

To access the identifier ``_id`` of the mongo documents it is used the
``->getId()`` method. It returns ``null`` if the document is new::

    $id = $article->getId();

New check
---------

To check if a document exists in the database you have to use the ``->isNew()``
method::

    $isNew = $article->isNew();

Fields
------

You have to use just the setters and getters::

    // setter
    $article->setTitle('Mandango rocks!');

    // getter
    $title = $article->getTitle();

References
----------

One
~~~

To assign references to one you simply have to assign the referenced document
to the reference's setter::

    $author = $mandango->create('Mandango\Author');
    $author->setName('pablodip');

    $article->setAuthor($author);

And to access the referenced objects is in the same way but with the getter::

    $author = $article->getAuthor();

If there is no a reference document it just returns ``null``.

.. note::
  The referenced document is queried automatically to the database if it has
  not been queried before.

Many
~~~~

To work with references to many it is used a ``Mandango\ReferenceGroup`` object,
to store the referenced documents.

Adding and deleting referenced documents::

    // retrieving the group
    $categories = $article->getCategories();

    // adding one by one
    $categories->add($category1);
    $categories->add($category2);

    // adding several at the same time
    $categories->add(array($category1, $category2));

    // shortcut to add from the document
    $article->addCategories($category); // one
    $article->addCategories($categories); // several

    // remove one by one
    $categories()->remove($category1);
    $categories()->remove($category2);

    // removing several at the same time
    $categories->remove(array($category1, $category2));

    // shortcut to remove from the document
    $article->removeCategories($category); // one
    $article->removeCategories($categories); // several

    // replacing (delete the existent ones and add the new ones)
    $article->getCategories()->replace(array($category2, $category4, $category6));

To work with the references to many the ``ReferenceGroup`` has some useful methods::

    // retrieving all documents (saved + add - removed)
    $categories = $categories->all();

    // count the documents
    $count = $categories->count();

The ``ReferenceGroup`` class implements the ``Countable`` and ``IteratorAggregate``
interfaces, so you can use them as well::

    // saved
    foreach ($categories as $category) {
        // ...
    }
    foreach ($article->getCategories() as $category) {
        // ...
    }

    // count
    $count = count($categories);
    $count = count($article->getCategories());

.. note::
  The ``ReferenceGroup`` has also an extremely useful *createQuery* method that we will see
  later of see the queries.

Embeddeds
---------

To work with the embeddeds is quite similar to work with the references.

One
~~~

::

    $article->setSource($source);

    $source = $article->getSource();

To many (many)
~~~~~~~~~~~~~~~

To work with the embeddeds many is used the ``EmbeddedGroup`` class instead, but
it works in the similar way that the ``ReferenceGroup`` one::

    $article->addComments($comment1);

    $article->removeComments($comment1);

    // ...


Relations
---------

The relations can only be accessed, and they return a document or a
``Mandango\Query`` object depending on the type::

    // one
    $phonenumber = $author->getPhonenumber(); // document

    // one_many
    $articles = $authors->getArticles(); // Mandango\Query

    // many_many
    $articles = $category->getArticles(); // Mandango\Query

.. note::
  We will see later why a query object is returned instead of an array
  of documents. A query object is much more useful.

Save and delete
---------------

To save and delete Mandango documents you can use the methods
``->save()`` y ``->delete()`` of the documents::

    // save
    $article->save();

    // delete
    $article->delete();

.. note::
  These methods are not in the embedded documents. You have to save always
  the documents through the main document.

Fluent interface
----------------

A fluent interface is implemented in the mandango documents to be able to work
easily with them::

    $author = $mandango->create('Model\Author');
    $author->setName('pablodip');
    $author->save();

    $article = $mandango->create('Model\Article');
    $article->setAuthor($author);
    $article->setTitle($title);
    $article->setContent($content);
    $article->save();

    // fluent interface
    $author = $mandango->create('Model\Author')->setName('pablodip')->save();

    $article = $mandango->create('Model\Article')
        ->setAuthor($author)
        ->setTitle($title)
        ->setContent($content)
        ->save()
    ;
