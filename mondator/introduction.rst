Introduction
============

Mondator is an easy and flexible class generator for PHP.

Requirements
------------

Mondator requires PHP 5.3.0 o upper.

Instalation
-----------

You can install Mondator directly, from git, svn.

Directly
^^^^^^^^

You can download directly from tar_ or zip_.

Git
^^^

http://github.com/mandango/mondator

Subversion
^^^^^^^^^^

There is an access through Subversion to the Git repository.

http://svn.github.com/mandango/mondator

Classes loader
--------------

To use Mondator you just have to add it namespace to a class loader::

    use Symfony\Component\HttpFoundation\UniversalClassLoader;

    $loader = new UniversalClassLoader();
    $loader->registerNamespaces(array(
        'Mandango\Mondator' => '/path/to/mondator/src',
    ));
    $loader->register();

.. _tar: http://github.com/mandango/mondator/tarball/master
.. _zip: http://github.com/mandango/mondator/zipball/master
