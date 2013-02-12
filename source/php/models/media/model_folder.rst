Model_Folder
############

.. php:class:: Model_Folder

	| Extend :php:class:`Model`.
	| Belongs to the namespace :php:ns:`Nos/Media`

Relations
*********

.. php:attr:: children

	* Relation type : :term:`has_many`.
	* Model : :php:class:`Model_Folder`

.. php:attr:: media

	* Relation type : :term:`has_many`.
	* Model : :php:class:`Model_Media`


.. php:attr:: parent

	* Relation type : :term:`belongs_to`.
	* Model : :php:class:`Model_Folder`

Behaviours
**********

* :php:class:`Tree <Orm_Behaviour_Tree>`
* :php:class:`Virtual path <Orm_Behaviour_Virtualpath>`

Methods
*******

.. php:method:: delete_from_disk()

	:returns: ``True`` or ``false`` depending on whether the deletion was successful.

	Delete folder recursively, media, sub-folders and itself.

.. php:method:: delete_public_cache()

	:returns: ``True`` or ``false`` depending on whether the deletion was successful.

	Delete all the public/cache entries (image thumbnails) for this folder

.. php:method:: path($file = '')

	:params string $file: A file name, right join to folder path in returns.
	:returns: Absolute folder path.

.. php:method:: count_media()

	:returns: Number of media contained in folder and its sub-folders.

.. php:method:: count_media_usage()

	:returns: Number of media contained in folder and his sub-folders that are in use.
