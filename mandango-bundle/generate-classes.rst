Generate Classes
================

The config classes are defined in the application and bundles config directories,
and they are defined in YAML_.

.. code-block:: yaml

    app/config/mandango/*.yml
    *Bundle/Resources/config/mandango/*.yml

You must to use the standard namespace *Model* in the classes, thus the bundles will be
reusable and coherent.

You don't need to use bundle namespace in your application classes:

.. code-block:: yaml

    # app/config/mandango/schema.yml
    Model\Article:
        fields:
            title:   string
            content: string

In the bundles is necessary to use the bundle name to separate the classes by bundle:

.. code-block:: yaml

    # src/Mandango/MandangoUserBundle/Resources/config/mandango/schema.yml
    Model\MandangoUserBundle\User:
        fields:
            username: string
            password: string

The classes are generated in the *src/Model* directory, and the bundle classes extend of
a bundle class before of the base class to be able to custom them in the bundles.

.. code-block:: yaml

    # application class
    Model\Article:
        Model\Base\Article

    # bundle class
    Model\MandangoUserBundle\User:
        Mandango\MandangoUserBundle\Model\User:
            Model\MandangoUserBundle\Base\User

To generate the classes is used the *mandango:generate* command:

.. code-block:: bash

    php app/console mandango:generate

.. _YAML: http://www.yaml.org
