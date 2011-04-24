Querying
========

The queries are made in Mandango through the document query classes,
which inherit from the ``Mandango\Query`` class, which is awesome.

The philosophy is simple: be as fast (lazy) and easy (human friendly) as
possible, and the results have been incredible.

The ``Mandango\Query`` class uses the mongo query syntax, so you don't have to learn
a new language to start to use it, although you can of course add much more
cool stuff on the top of it.

Let's see how it works::

    $query = \Model\Article::repository()->query(); // Model\ArticleQuery
    $query = \Model\Article::repository()->query($criteria);

    // shortcut
    $query = \Model\Article::query();
    $query = \Model\Article::query($criteria);

    // methods (fluent interface)
    $query
        ->criteria(array('is_active' => true))
        ->fields(array('title' => 1))
        ->sort(array('date' => -1))
        ->limit(10)
        ->skip(25)
        ->batchSize(3)
        ->hint(array('date' => 1))
        ->snapshot(true)
        ->timeout(100)
    ;

    // the real query is only executed in these cases
    foreach ($query as $result) { // iterating (IteratorAggregate interface)
    }
    $articles = $query->all(); // retrieving all results explicitly
    $article = $query->one(); // retrieving one result  explicitly

    // counting results (directly, without hydrate)
    $nb = $query->count();
    $nb = count($query); // Countable interface

    // deleting results (directly, without hydrate)
    $articles->remove();

Applying logic
--------------

Like you have seen, the queries only execute the real database's queries when
you iterate or when you do it explicitly. That means that you can play with the
queries before they really query
something::

    $query = \Model\Article::query();

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

Making useful methods
---------------------

If you query the same criteria and options usually, you can refactor it in a
query method::

    public function oneBySlug($slug)
    {
        return $this->criteria(array('slug' => $slug))->one();
    }

    $article = \Model\Article::query()->oneBySlug($slug);

References many
---------------

Please, remember how :doc:`references many work </mandango/working-with-objects>`.

The ``Mandango\\ReferenceGroup`` class has a ``query`` method that just returns a mandango
query object to query the referenced documents. So, as the mandango query class
is awesome, you can also make awesome stuff with it::

    // query and returns all categories
    $categories = $article->getCategories()->saved();
    // just returns a query object to query the referenced categories:
    //     array('_id' => array('$in' => $categoryIds))
    $categories = $article->getCategories()->query();

    // using the query, applying any logic
    $categories->mergeCriteria(array('name' => new \MongoRegex('/^A/')));
    $categories->sort(array('name' => 1));
    $categories->limit(10)->skip(5);

Relations
---------

The relations many just return a query object, so you can use it in the same way::

    $articles = $author->getCategories();
    $articles->mergeCriteria($criteria);
    $nbArticles = $articles->count();
    $articles->remove();

Collection
----------

You can also use the mongo collection directly to do the customized operations
you need::

    $collection = \Model\Article::collection();
