Validation
==========

To define the validation of your Mandango's documents you can use the config classes:

.. code-block:: yaml

    Model\Author:
        fields:
            name: { type: string, validation: [NotBlank: ~, Type: string] }

    Model\Category:
        fields:
            name: { type: string, validation: [NotBlank: ~, Type: string] }

    Model\Post:
        fields:
            title:       { type: string, validation: [NotBlank: ~, Type: string] }
            summary:     { type: string, validation: [NotBlank: ~, Type: string] }
            content:     { type: string, validation: [NotBlank: ~, Type: string] }
            publishedAt: { type: date, validation: [DateTime: ~] }
            isActive:    { type: boolean, default: false, validation: [Type: boolean] }
        referencesOne:
            author: { class: Model\PablodipMandangoBlogBundle\Author, validation: [NotNull: ~]  }
        referencesMany:
            categories: { class: Model\PablodipMandangoBlogBundle\Category }
        embeddedsOne:
            source: { class: Model\Source, validation: [NotNull: ~] }

    Model\Source:
        isEmbedded: true
        fields:
            name: { type: string, validation: [NotBlank: ~] }
            url:  { type: string }

And generate the classes:

.. code-block:: bash

    php app/console mandango:generate
