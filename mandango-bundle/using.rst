Using
=====

You can use Mandango in a normal way.

You can access to the mandango in the container::

    $mandango = $container->get('mandango');

The mandango is initialized also automatically (in a lazy way) if you use some
functionality in the documents that require it::

    // creating
    $article = $mandango->create('Model\Article');
    $article->setTitle($title);
    $article->save();

    // quering
    $articles = $mandango->getRepository('Model\Article')->createQuery();

    // ...
