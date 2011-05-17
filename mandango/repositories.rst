Repositories
============

The repositories perform general functions of documents.

They are obtained through the ``getRepository`` method of the mandango::

    $articleRepository = $mandango->getRepository('Model\Article');
    $authorRepository = $mandango->getRepository('Model\Author');

.. note::
  The embedded documents don't have repositories.

Finding by ID
-------------

The repositories implement the ``findById`` and ``findOneById`` methods to find
documents by id::

    // one
    $article = $articleRepository->findOneById($id);

    // several
    $articles = $articleRepository->findById(array($id1, $id2, $id3));

Mandango implements the IdentityMap_ pattern, so when you find a document
and the document has been found already, the same document is returned.

.. note::
  You can use a ``\MongoId`` object or a string.

Saving Documents
----------------

To save documents you can also use the ``->save()`` method of the
repositories, which is the one that the save method of the documents uses
internally::

    // saving a document
    $articleRepository->save($article);

    // saving several documents
   $articleRepository->save(array($article1, $article2));

.. note::
  To insert documents it is used the batchInsert_ method,
  and to update the `atomic operations`_ one,
  so both functions are done in a **very efficient** way.

.. tip::
  Inserting document through the repository directly is useful when you
  have to insert more than one document. This way all the operation is done
  in the same batchInsert.

Deleting Documents
------------------

The same logic that with the save method is applied here::

    // deleting a document
    $articleRepository->delete($article);

    // deleting several documents
    $articleRepository->delete(array($article1, $article2));

.. note::
  It is also very useful **to delete a lot of documents** from the repository, because
  it uses the ``$in`` operator.

Group
-----

The repositories have the ``group`` method, which is a shortcut to the ``group``
method of the repositories::

    $result = $repository->group($keys, $initial, $reduce);
    $result = $repository->group($keys, $initial, $reduce, $options); // options optional

    // the same as
    $result = $repository
        ->getCollection()
        ->group($keys, $initial, $reduce, $options)
    ;

Distinct
--------

The repositories have the ``distinct`` method to execute distinct commands easily::

    $result = $repository->distinct($field);
    $result = $repository->distinct($field, $query); // query optional

    // the same as
    $result = $repository
        ->getCollection()
        ->getMongoDB()
        ->command(array(
            'distinct' => $repository->getCollectionName(),
            'key'      => $field,
            'query'    => $query,
        ))
    ;

Map Reduce
----------

The repositories have the ``mapReduce`` method to execute map reduces commands easily::

    $result = $repository->mapReduce($map, $reduce, $out);
    $result = $repository->mapReduce($map, $reduce, $out, $query, $options); // more args (optionals)

    // the same as
    $result = $repository
        ->getCollection()
        ->getMongoDB()
        ->command(array_merge($options, array(
            'mapreduce' => $repository->getCollectionName(),
            'map'       => $map,
            'reduce'    => $reduce,
            'out'       => $out,
            'query'     => $query,
        )))
    ;

Connection
----------

You can get the mandango connection of the documents from the repository
through the ``->getConnection()`` method::

    $connection = $articleRepository->getConnection();

Collection
----------

You can also obtain the mongo collection to perform operations directly
with the ``->getConnection()`` method::

    $collection = \Model\Article::getRepository()->getCollection();

.. _IdentityMap: http://martinfowler.com/eaaCatalog/identityMap.html
.. _batchInsert: http://www.php.net/manual/en/mongocollection.batchinsert.php
.. _atomic operations: http://www.mongodb.org/display/DOCS/Atomic+Operations
.. _$in: http://www.mongodb.org/display/DOCS/Advanced+Queries#AdvancedQueries-%24in
