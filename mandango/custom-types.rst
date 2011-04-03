Custom types
============

You can use custom types to accomplish the mapping of documents.

To do that you have to create a mandango type and add it to the mandango types' container.

Creating custom types
---------------------

A mandango type is a class that inherits from *Mandango\Type\Type* and
implements its four abstract methods.

Let's see an example::

    namespace MyProject\Type;

    use Mandango\Type\Type;

    class MyType extends Type
    {
        public function toMongo($value)
        {
            return (string) $value;
        }

        public function toPHP($value)
        {
            return (string) $value;
        }

        public function toMongoInString()
        {
            return '%to% = (string) %from%;';
        }

        public function toPHPInString()
        {
            return '%to% = (string) %from%;';
        }
    }

.. hint::
  **Four abstract methods**:

  * **toMongo**: convert a PHP value to Mongo
  * **toPHP**: convert a Mongo value to PHP
  * **toMongoInString**: convert a PHP value to Mongo in string (to generate the classes)
  * **toPHPInString**: convert a Mongo value to PHP in string (to generate the classes)

Mandango types' container
-------------------------

Once you have the type you just have to add it to the mandango types' container,
in order to be able to use it::

    use Mandango\Type\Container;

    Container::addType('my_type', 'MyProject\Type\MyType');

.. note::
  It is only necessary to do this when the classes are generated, not when they
  are going to be used.
