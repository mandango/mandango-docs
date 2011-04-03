Extensions
==========

The extensions are the ones in charge of indicating, from the config classes,
which classes will be generated, how they will be, and where they will be.
This is done through the definition and outputs, as we have seen before.

Definitions with Outputs
------------------------

Each definition must be an output. For this it is used the
*Mandango\Mondator\Definition* class instead::

    use Mandango\Mondator\Definition;
    use Mandango\Mondator\Output;

    $output = new Output($modeldir);
    $definition = new Definition($class, $output);

    // use the definition in the same way that a regular definition
    // ...

Creating Extensions
-------------------

The extensions are classes that inherit from *Mandango\Mondator\Extension* and
implement the protected *->doClassProcess()* method.

The *->doClassProcess()* method is the place where the config classes
are processed and are created or modified definitions and/or outputs. The
processing of the config classes is made one by one.

.. hint::
  **Variables of the classes meanwhile process**:

  * **$this->class**: class name
  * **$this->configClass**: class data
  * **$this->definitions**: the definitions container

The extensions also accept options to be able to customize its behavior.
The options are added in the protected *->setup()* method, and can be required
or optional.

.. hint::
  **Methods for adding options**:

  * **$this->addRequiredOption($name)**: adds a required option
  * **$this->addOption($name, $defaultValue)**: adds an optional option
  * **$this->addRequiredOptions($options)**: add several required options (name as value)
  * **$this->addOptions($options)**: adds several optional options (name as key and value by default as value)

Example
^^^^^^^

We are going to do an example that creates definitions of *document* and
**document_base**, one inherited from the other, and adds to the base class properties,
setters and getters of the fields that have class configurations::

    namespace MyProject\Extension;

    use Mandango\Mondator\Definition;
    use Mandango\Mondator\Definition\Property;
    use Mandango\Mondator\Definition\Method;
    use Mandango\Mondator\Extension;
    use Mandango\Mondator\Output;
    use Mandango\Inflector;

    class DocumentExtension extends Extension
    {
        protected method setup()
        {
            $this->addRequiredOption('output');
            $this->addOption('process_fields', true);
        }

        protected method doProcess()
        {
            // document
            $output = new Output($this->getOption('output'));
            $this->definitions['document'] = new Definition($this->class, $output);
            $this->definitions['document']->setParentClass($this->class.'Base');

            // document_base
            $output = new Output($this->getOption('output').'/Base');
            $this->definitions['document_base'] = new Definition($this->class.'Base', $output);
            $this->definitions['document_base']->setIsAbstract(true);

            // fields
            if ($this->getOption('process_fields')) {
                if (isset($this->configClass['fields'])) {
                    foreach ($this->configClass['fields'] as $name)
                    {
                        // property
                        $property = new Property('protected', $name, null);
                        $this->container['document_base']->addProperty($property);

                        // setter
                        $method = new Method('public', 'set'.Inflector::camelize($name), '$value', <<<EOF
            \$this->$name = \$value;
    EOF
                        );
                        $this->container['document_base']->addMethod($method);

                        // getter
                        $method = new Method('public', 'get'.Inflector::camelize($name), '', <<<EOF
            return \$this->$name;
    EOF
                        );
                        $this->container['document_base']->addMethod($method);
                    }
                }
            }
        }
    }
