Behaviors
=========

The behaviors are one of the best things of Mandango. They are just extensions,
but to be used in classes **independently** instead in all the document classes,
which is a great advantage.

What can a behavior do? All that an extension can, but translated to the
Mandango's language:

.. hint::
  * Change (add/edit/remove) config classes (mapping):

    * Fields
    * References
    * Embeddeds
    * Relations
    * Events
    * Indexes
    * ...

  * Add new document classes
  * Add new classes to generate for each document class
  * Change (add/edit/remove) methods and properties to generated classes:

    * Documents
    * Repositories
    * Queries
    * ...

Using
-----

To use behaviors you have to define them in the config classes, the class and
the options if they exist::

    array(
        'Model\Article' => array(
            // ...
            'behaviors' => array(
                // simple
                array('class' => 'Mandango\Behavior\Timestampable'),
                // with options
                array(
                    'class' => 'Mandango\Behavior\Sluggable',
                    'options' => array('fieldFrom' => 'title'),
                ),
            ),
        ),
    );

By default
----------

If you want to use any behavior by default in all document classes, you can
define it in the Core extension options::

    $extensions[] = new \Mandango\Extension\Core(array(
        // ...
        'default_behaviors' => array(
            array(
                'class' => 'Mandango\Behavior\Timestampable',
                'options' => array('updatedEnabled' => false),
            )),
        ),
    ));

And if you want to use any of them only in the main documents::

    $extensions[] = new \Mandango\Extension\Core(array(
        // ...
        'default_behaviors' => array(
            array(
                'in_embeddeds' => false,
                'class' => 'Mandango\Behavior\Timestampable',
                'options' => array('updatedEnabled' => false),
            )),
        ),
    ));

Creating behaviors
------------------

Let's see the Timestampable behabior::

    namespace Mandango\Behavior;

    use Mandango\Inflector;
    use Mandango\Mondator\ClassExtension;
    use Mandango\Mondator\Definition\Method;

    /**
     * Timestampable.
     *
     * @author Pablo Díez <pablodip@gmail.com>
     */
    class Timestampable extends ClassExtension
    {
        /**
         * {@inheritdoc}
         */
        protected function setUp()
        {
            $this->addOptions(array(
                'createdEnabled' => true,
                'createdField'   => 'created_at',
                'updatedEnabled' => true,
                'updatedField'   => 'updated_at',
            ));
        }

        /**
         * {@inheritdoc}
         */
        protected function doConfigClassProcess()
        {
            // created
            if ($this->getOption('createdEnabled')) {
                $this->configClass['fields'][$this->getOption('createdField')] = 'date';
                $this->configClass['events']['preInsert'][] = 'updateTimestampableCreated';
            }

            // updated
            if ($this->getOption('updatedEnabled')) {
                $this->configClass['fields'][$this->getOption('updatedField')] = 'date';
                $this->configClass['events']['preUpdate'][] = 'updateTimestampableUpdated';
            }
        }

        /**
         * {@inheritdoc}
         */
        protected function doClassProcess()
        {
            // created
            if ($this->getOption('createdEnabled')) {
                $fieldSetter = 'set'.Inflector::camelize($this->getOption('createdField'));

                $method = new Method('protected', 'updateTimestampableCreated', '', <<<EOF
            \$this->$fieldSetter(new \DateTime());
    EOF
                );
                $this->definitions['document_base']->addMethod($method);
            }

            // updated
            if ($this->getOption('updatedEnabled')) {
                $fieldSetter = 'set'.Inflector::camelize($this->getOption('updatedField'));

                $method = new Method('protected', 'updateTimestampableUpdated', '', <<<EOF
            \$this->$fieldSetter(new \DateTime());
    EOF
                );
                $this->definitions['document_base']->addMethod($method);
            }
        }
    }
