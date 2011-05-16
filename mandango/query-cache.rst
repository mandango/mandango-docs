Query Cache
===========

The Mandango's Query Cache is one of the most impressive features of Mandango,
because it multiplies the speed of your application and database.

If we see the optimization page in the MongoDB documentation, we can see that
`querying only the data you use`_ you get a much better performance. And this
is what the Query Cache does, but automatically, which is a huge advantage,
because developers usually don't ;)

Well, let's see an example::

    array(
        'Model\User' => array(
            'fields' => array(
                'username' => 'string',
                'password' => 'string',
                'salt'     => 'string',
                // ... to 100
            ),
        ),
    );

What happen when you query an user with 100 fields? Usually your library queries
the full user to the database and then you have the full user. So you query
the full even if you are going to work only with few of its fields.

What does Mandango do? Mandango saves automatically the fields you use in each
query and it queries only those fields::

    // first time
    // fields: array('_id' => 1)
    $user = $userRepository->createQuery()->one();
    $user->getUsername() // query the username, and it saves the salt is queried
    $user->getSalt() // query the salt, and it saves the salt is queried

    // second time
    // fields: array('_id' => 1, 'username' => 1, 'salt' => 1)
    $user = $userRepository->createQuery()->one();

This way it does ever query the full user, so the speed is incredible.

Cases
-----

Think in these cases:

Big fields
^^^^^^^^^^

It's a really big advantage if you don't query the big fields of your documents
if you don't need them. A huge improvement for the application (processing and
memory) and for you database and network::

    array(
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'summary' => 'string',
                'content' => 'string', // huge field
                'image'   => 'bin_data' // file
            ),
        ),
    );

    $articles = $articleRepository->createQuery()->all();
    // array('_id' => 1, 'title' => 1, 'summary' => 1)
    // without the content and image big fields, you are not using them!

    foreach ($articles as $article) {
        echo $article->getTitle();
        echo $article->getSummary();
    }

Collection with Embeddeds
^^^^^^^^^^^^^^^^^^^^^^^^^

This works also with embeddeds one::

    array(
        'Model\Article' => array(
            // ...
            'embeddedsOne' => array(
                'source' => array('class' => 'Model\Source'),
            ),
        ),
        'Model\Source' => array(
            'isEmbedded' => true,
            'fields' => array(
                'name' => 'string',
                'url'  => 'string',
            ),
        ),
    );

    $article = $articleRepository->createQuery()->one();
    // array('_id' => 1, 'source.name' => 1)

    $article->getSource()->getName();

Taking data from references
^^^^^^^^^^^^^^^^^^^^^^^^^^^

A normal use case is to take data from references::

    $article = $articleRepository->createQuery()->one();
    // array('_id' => 1)

    $article->getAuthor()->getName();
    // to author: array('_id' => 1, 'name' => 1)
    // it does not matter how many fields the author has!

References
^^^^^^^^^^

The Query Cache works also with the Mandango's references query, which is great.
This is done automatically when you access to a reference::

    $articles = $articleRepository->createQuery()/*->references('author)*/->all();
    foreach ($articles as $article) {
        $article->getAuthor(); // queried!
    }

Conclusion
----------

Like you have seen, this is indeed a really good feature for your
application's performance, and you even don't need to do anything apart from
use Mandango :)

.. _querying only the data you use: http://www.mongodb.org/display/DOCS/Optimization#Optimization-Optimization%233%3ASelectonlyrelevantfields
