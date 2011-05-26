Querying
========

The queries are made in Mandango through the document query classes,
which inherit from the ``Mandango\Query`` class, which is awesome.

The philosophy is simple: be as fast (lazy) and easy (human friendly) as
possible, and the results are incredible.

The ``Mandango\Query`` class uses the mongo query syntax, so you don't have to learn
a new language to start to use it, although you can of course add much more
cool stuff on the top of it.

Let's see how it works::

    $query = $articleRepository->createQuery(); // Model\ArticleQuery
    $query = $articleRepository->createQuery($criteria);

    // methods (fluent interface)
    $query
        ->criteria(array('is_active' => true))
        ->fields(array('title' => 1))
        ->sort(array('date' => -1))
        ->limit(10)
        ->skip(25)
        ->batchSize(3)
        ->hint(array('date' => 1))
        ->slaveOkay(true)
        ->snapshot(true)
        ->timeout(100)

        ->references() // Mandango's extra
    ;

    // the real query is only executed in these cases
    foreach ($query as $result) { // iterating (IteratorAggregate interface)
    }
    $articles = $query->all(); // retrieving all results explicitly
    $article = $query->one(); // retrieving one result  explicitly

    // counting results (directly, without hydrate)
    $nb = $query->count();
    $nb = count($query); // Countable interface

References
----------

MongoDB does not have joins, but Mandango has the best and most efficient way
to solve this feature. Usually when you access to a reference, the reference is
queried, so if you have to access to several references, you have to make
several queries. What does Mandango do? Simply to group the references queries
in only one query::

    // queries articles
    $articles = $articleRepository->createQuery()
        ->all()
    ;
    foreach ($articles as $article) {
        // query each author, so the number of queries depends on the different authors
        $article->getAuthor();
    }

    // queries articles and their authors
    // only two queries, no matter how many authors there are
    // authors queried with array('_id' => '$in' => $authorIds)
    $articles = $articleRepository->createQuery()
        ->references('author')
        ->all()
    ;
    foreach ($articles as $article) {
        $article->getAuthor(); // queried!
    }

Applying logic
--------------

Like you have seen, the queries only execute the real database's queries when
you iterate or when you do it explicitly. That means that you can play with the
queries before they really query something::

    $query = $articleRepository->createQuery();

    if ($criteria) {
        $query->criteria($criteria);
    }
    if ($sort) {
        $query->sort($sort);
    }

    // you can apply the logic that you want before to query...

Merging criteria
----------------

When we apply logic depending on parameters, sometimes we want just modify part
of the criteria. You can use the ``->mergeCriteria()`` method for this::

    // normal
    $query->criteria(array('author' => $author->getId));
    if ($active) {
        $query->criteria(array_merge($query->getCriteria(), array('is_active' => true)));
    }

    // merging
    $query->criteria(array('author' => $author->getId));
    if ($active) {
        $query->mergeCriteria(array('is_active' => true));
    }

Reusing logic
-------------

A query class is generated for each document class, so you can save logic on it::

    // Model\ArticleQuery
    public function active()
    {
        $this->mergeCriteria(array('is_active' => true));
    }

    // using
    $query->criteria(array('author' => $author->getId));
    if ($active) {
        $query->active();
    }

References many
---------------

Please, remember how :doc:`references many work </mandango/working-with-objects>`.

The ``Mandango\\ReferenceGroup`` class has a ``createQuery`` method that just returns a
query object to query the referenced documents. So, as the mandango query class
is awesome, you can also make awesome stuff with it::

    // query and returns all categories
    $categories = $article->getCategories()->all();
    // just returns a query object to query the referenced categories:
    //     array('_id' => array('$in' => $categoryIds))
    $categories = $article->getCategories()->createQuery();

    // using the query, applying any logic
    $categories->mergeCriteria(array('name' => new \MongoRegex('/^A/')));
    $categories->sort(array('name' => 1));
    $categories->limit(10)->skip(5);
    $nbCategories = $categories->count();

Relations
---------

The relations many just return a query object, so you can use it in the same way::

    $articles = $author->getCategories();
    $articles->mergeCriteria($criteria);
    $nbArticles = $articles->count();

Collection
----------

You can also use the mongo collection directly to do the customized operations
you need::

    $collection = $articleRepository->getCollection();
