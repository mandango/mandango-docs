Security
========

The MandangoBundle comes with the *MandangoUserProvider* provider to integrate
your Mandango's documents with the Symfony2 security layer.

.. code-block:: yaml

    services:
        security.provider.mandango:
            class: Mandango\MandangoBundle\Security\MandangoUserProvider
            arguments: [@mandango, Model\User, username]

    security:
        providers:
            mandango:
                id: security.provider.mandango
