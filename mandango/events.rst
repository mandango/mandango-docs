Events
=======

Mandango provides to execute methods in the documents before and after
performing certain actions:

.. hint::
  * **preInsert**: before inserting
  * **postInsert**: after inserting
  * **preUpdate**: before updating
  * **postUpdate**: after updating
  * **preDelete**: before deleting
  * **postDelete**: after deleting

You just have to define the document methods you want to execute and in what
action in the config classes::

    array(
        'Model\Article' => array(
            // ...
            'events' => array(
                'preInsert' => array('updateCreatedAtBeforeInsert'),
                'preUpdate' => array('updateUpdatedAtBeforeUpdate'),
            ),
        ),
    );

And write the methods in the document class::

    namespace Model\;

    class Article extends \Model\Base\Article
    {
        public function updateCreatedAtBeforeInsert()
        {
            $this->setCreatedAt(new DateTime());
        }

        public function updateUpdatedAtBeforeUpdate()
        {
            $this->setUpdatedAt(new DateTime());
        }
    }

.. note::
  The document methods have to be public.

This feature does not have any penalty in performance if you don't use it,
because the code to execute the methods is generated only if you indicate it
explicitly. And if you use it, the performance penalty just depends on the
methods you use.
