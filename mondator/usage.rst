Usage
=====

Mondator uses *config classes* and *extensions* to generate the
classes. The config classes have the information about themselves,
and the extensions process those data and indicate which classes have to be
generated, where and how they must be.

Mondator is extremely flexible, because the classes that are generated from the
config classes depend on the extensions that you have assigned them.

Mandango uses an extension of Mondator to generate the classes of documents,
repositories and queries.

A simple example
----------------

We are going to do a simple example with Mondator using Mandango, this way
you will see step by step what you will have to do to use it and you will
understand how it works.

Create a Mondator
~~~~~~~~~~~~~~~~~

::

    use Mandango\Mondator\Mondator;

    $mondator = new Mondator();

Assign the Config Classes
~~~~~~~~~~~~~~~~~~~~~~~~~

The config classes contain the information of the classes.::

    $mondator->setConfigClasses(array(
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
        ),
    ));

.. tip::
  You can use a language like `YAML`_ to work easily with the config classes.

Assign the extensions
~~~~~~~~~~~~~~~~~~~~~

The extensions are the ones ordered to process the config classes and
generate class definitions and outputs. The definitions indicate which
classes will be generated, and the outputs where they will be generated.

The order in which the extensions are assigned is the order in which they are
processed, so it is very important that it is the right order.

The extensions also accept options to be able to customize its operation::

    $mondator->setExtensions(array(
        new Mandango\Extension\Core(array(
            'metadata_class' => 'Model\Mapping\Metadata',
            'metadata_output' => $modelDir.'/Mapping',
            'default_output' => $modelDir,
        )),
    ));

.. note::
  Adding more extensions you will be able to customize the generated classes
  as much as you want, or event generate more classes.

Process
~~~~~~~

Once you have assigned the config classes and extensions, you only have to
process the mondator to generate the files of each classes::

    $mondator->process();

A full example
--------------

Let's see a full example, which you can use to start using Mandango::

    $mandangoDir = '/path/to/mandango';
    $modelDir  = '/path/to/Model';

    // classes loader
    require_once($mandangoDir.'/vendor/symfony/src/Symony/Component/ClassLoader/UniversalClassLoader.php');

    use Symfony\Component\ClassLoader\UniversalClassLoader;

    $loader = new UniversalClassLoader();
    $loader->registerNamespaces(array(
        'Mandango' => $mandangoDir.'/src/',
        'Mondator' => $mandangoDir.'/vendor/mondator/src',
        'Model'    => dirname($modelDir),
    ));
    $loader->register();

    // start Mondator
    use Mandango\Mondator\Mondator;

    $mondator = new Mondator();

    // assign the config classes
    $mondator->setConfigClasses(array(
        'Model\Article' => array(
            'fields' => array(
                'title'   => 'string',
                'content' => 'string',
            ),
        ),
    ));

    // assign extensions
    $mondator->setExtensions(array(
        new Mandango\Extension\Core(array(
            'metadata_class' => 'Model\Mapping\Metadata',
            'metadta_output' => $modelDir.'/Mapping',
            'default_output' => $modelDir,
        )),
    ));

    // process
    $mondator->process();

If you take a look at the generated files, you will see that there are empty
classes that you can customize, and others *Base* that **you must not touch**
because they are **overwritten** every time the mondator is processed.

.. _YAML: http://www.yaml.org
