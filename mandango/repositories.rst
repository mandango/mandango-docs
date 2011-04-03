Repositories
============

The repositories perform general functions of documents.

They are obtained through the *repository* static method of the document class::

    $articleRepository = \Model\Article::repository();
    $authorRepository = \Model\Author::repository();

.. note::
  The embedded documents don't have repositories.

Saving Documents
------------------

To save documents you can also use the *->save()* method of the
repositories, which is the one that the save method of the documents use
internally::

    // saving a document
    $articleRepository->save($article);

    // saving several documents
    $articleRepository->save(array($article1, $article2));

.. note::
  To insert documents it is used the method batchInsert_,
  and to update the `atomic operations`_ one,
  so both functions are done in a **very efficient** way.

.. tip::
  Inserting document through the repository directly is useful when you
  have to insert more then one document. This way all the operation is done
  in the same batchInsert.

Deleting Documents
------------------

The same logic that with the save method is applied here::

    // deleting a document
    $articleRepository->delete($article);

    // deleting several documents
    $articleRepository->delete(array($article1, $article2));

    // access from the Mandango
    $mandango->delete('Model\Document\Article', array($article1, $article2));

.. note::
  It is also very useful **to delete a lot of documents** from the repository, because
  it uses the $in_ operator.

Connection
----------

You can get the mandango connection of the documents from the repository
through the *->getConnection()* method::

    $connection = $articleRepository->getConnection();

Collection
----------

You can also obtain the mongo collection to perform operations directly
with the *->getConnection()* method::

    $collection = $articleRepository->getCollection();

    // shortcut
    $collection = \Model\Article::collection();

.. _batchInsert: http://www.php.net/manual/en/mongocollection.batchinsert.php
.. _atomic operations: http://www.mongodb.org/display/DOCS/Atomic+Operations
.. _$in: http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries-%24in
