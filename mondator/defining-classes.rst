Defining Classes
================

The definitions are used to indicate internally in the extensions which classes
are going to be generated and how they will be.

Classes
-------

The classes are defined with the class
*Mandango\\Mondator\\Definition\\Definition*::

    use Mandango\Mondator\Definition\Definition;

    // constructor: class
    $definition = new Definition($class);

    // class
    $definition->setClass('Model\Article');
    $class = $definition->getClass();

    // namespace (only return) (null if it doesn't have)
    $namespace = $definition->getNamespace();

    // classname (only return)
    $className = $definition->getClassName();

    // parent class (full class)
    $definition->setParentClass('\Model\Base\Article');
    $parentClass $definition->getParentClass();

    // interfaces
    $definition->addInterface('\ArrayAccess');
    $interfaces = $definition->getInterfaces();

    // abstract
    $definition->setIsAbstract(true);
    $isAbstract = $definition->getIsAbstract();

    // DocComment
    $definition->setDocComment(<<<EOF
    /**
     * Article Document.
     */
    EOF
    );
    $docComment = $definition->getDocComment();

Methods
^^^^^^^

The methods use the class *Mandango\Mondator\Definition\Method*::

    use Mandango\Mondator\Definition\Method;

    // constructor: visibility, name, arguments, code
    $method = new Method('public', 'myMethod', '$var1, $var2', <<<EOF
            return \$var1 + \$var2;
    EOF
    );

    // visibility
    $method->setVisibility('protected');
    $visibility = $method->getVisibility();

    // name
    $method->setName('mySumMethod');
    $name = $method->getName();

    // arguments
    $method->setArguments('$sum1, $sum2');
    $arguments = $method->getArguments();

    // code
    $method->setCode(<<<EOF
            return \$sum1 + \$sum2;
    EOF
    );
    $code = $method->getCode();

    // static
    $method->setIsStatic(true);
    $isStatic = $method->getIsStatic();

    // abstract
    $method->setIsAbstract(true);
    $isAbstract = $method->getIsAbstract();

    // docComment
    $method->setDocComment(<<<EOF
        /**
         * Sum two numbers.
         *
         * @param mixed $sum1 The Sum1.
         * @param mixed $sum2 The Sum2.
         *
         * @return mixed The result.
         */
    EOF
    );
    $docComment = $method->getDocComment();

The methods are assigned to the definitions::

    // assign one
    $definition->addMethod($method);

    // replace (deletes all that existed previously)
    $definition->setMethods(array($method1, $method2));

    // return all
    $methods = $definition->getMethods();

Properties
^^^^^^^^^^

The properties use the class *Mandango\Mondator\Definition\Property*::

    use Mandango\Mondator\Definition\Property;

    // constructor: visibility, name, value
    $property = new Property('public', 'myProperty', 'value');

    // visibility
    $property->setVisibility('protected');
    $visibility = $property->getVisibility();

    // name
    $property->setName('myCustomProperty');
    $name = $property->getName();

    // value
    $property->setValue('someValue');
    $value = $property->getValue();

    // static
    $property->setIsStatic(true);
    $isStatic = $property->getIsStatic();

    // docComment
    $property->setDocComment(<<<EOF
        /**
         * MyCustomProperty.
         */
    EOF
    );
    $docComment = $property->getDocComment();

The properties are assigned also to the definitions::

    // assign one
    $definition->addProperty($property);

    // replace (deletes all that existed previously)
    $definition->setProperties(array($property1, $property2));

    // return all
    $properties = $definition->getProperties();
