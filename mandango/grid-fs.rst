GridFS
======

With Mandango you will be able to store files in Mongo using GridFS_ through
MongoGridFS_ objects of the PHP driver very easily.

For it you have to map them as files::

    array(
        'Model\Image' => array(
            'is_file' => true,
            'fields' => array(
                'name' => 'string',
            ),
        ),
    );

When doing it the field **file** is automatically added, that is the one
in which the file is saved::

    $image = new \Model\Image();

    // as file
    $image->setFile('/path/to/file');

    // as bytes
    $image->setFile(file_get_contents('/path/to/file'));

    $image->save();

.. warning::
  The file **can't be updated**. The remaining mapped field can be updated.

And to retrieve them it is done the same way::

    $image = \Model\Image::query()->one();

    $image->getFile() // \MongoGridFSFile

    $bytes = $image->getFile()->getBytes();

.. _GridFS: http://www.mongodb.org/display/DOCS/GridFS
.. _MongoGridFS: http://php.net/manual/en/class.mongogridfs.php
