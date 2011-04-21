Introduction
============

The MandangoBundle is the bundle to use Mandango with Symfony2_.

Installation
------------

Add Mandango and Mondator to your vendors:

.. code-block:: bash

    git submodule add git://github.com/mandango/mandango vendor/mandango
    git submodule add git://github.com/mandango/mondator vendor/mondator

Add the MandangoBundle:

.. code-block:: bash

    git submodule add git://github.com/mandango/MandangoBundle vendor/bundles/Mandango/MandangoBundle

Add all of them and the Model namespace to your autoload::

    // app/autoload.php
    $loader->registerNamespaces(array(
        // ...
        'Mandango\MandangoBundle' => __DIR__.'/../vendor/bundles',
        'Mandango\Mondator'       => __DIR__.'/../vendor/mondator/src',
        'Mandango'                => __DIR__.'/../vendor/mandango/src',
        'Model'                   => __DIR__.'/../src/',
        // ...
    ));

Add the MandangoBundle to your application kernel::

    // app/AppKernel.php
    public function registerBundles()
    {
        return array(
            // ...
            new Mandango\MandangoBundle\MandangoBundle(),
            // ...
        );
    }

Configuration
-------------

Add Mandango to your configuration:

.. code-block:: yaml

    # app/config/config.yml
    mandango:
        default_connection: local
        connections:
            local:
                server:   mongodb://localhost:27017
                database: symfony2_local_%kernel.environment%

Activate the profiler in the developing environment:

.. code-block:: yaml

    # app/config/config_dev.yml
    mandango:
        logging: true

.. _Symfony2: http://www.symfony.com
