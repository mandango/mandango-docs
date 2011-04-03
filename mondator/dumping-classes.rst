Dumping Classes
===============

To dump definitions it is used the Mondator's Dumper.

The Mondator's Dumper dumps in a string the PHP code of the class of a
definition. This PHP code is generated according to the options in the
definition and the methods and properties that it has::

    use Mandango\Mondator\Dumper;

    // constructor: definition
    $dumper = new Dumper($definition);

    $classCode = $dumper->dump();

Later you can use the code of the class as you want, writing it in a
file, for example::

    file_put_contents($file, $classCode);
