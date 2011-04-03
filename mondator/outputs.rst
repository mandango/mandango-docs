Outputs
=======

The outputs are used to indicate the output file of the classes and if they
are overwritten or not::

    use Mandango\Mondator\Output;

    $modelDir = '/path/to/your/model/dir';

    // without overwriting
    $output = new Output($modelDir'/Article.php');

    // overwriting (true as second argument)
    $output = new Output($modelDir'/Base/Article.php', true);

..  note::
  If it is overwritten or not depends if the user is going to customize
  the class, so the changes made are not lost.
