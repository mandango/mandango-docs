Forms
=====

The MandangoBundle is integrated with the new Symfony2 Forms::

    class PostType extends AbstractType
    {
        public function buildForm(FormBuilder $builder, array $options)
        {
            $builder
                ->add('title')
                ->add('summary', 'textarea')
                ->add('content', 'textarea')
                ->add('publishedAt')
                ->add('isActive')
                // referencesOne
                ->add('author')
                // referencesMany
                ->add('categories')
            ;
        }

        public function getDefaultOptions(array $options)
        {
            return array(
                'data_class' => 'Model\Post',
            );
        }

        public function getIdentifier()
        {
            return 'post';
        }
    }
