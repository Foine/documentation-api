Model_Link
###########

.. php:namespace:: Nos\Media

.. php:class:: Model_Link

	| Link between :php:class:`Model_Media` and :php:class:`Nos\\Orm\\Model`.
	| Extend :php:class:`Nos\\Orm\\Model`.

Columns
*******

:medil_id: Model_Link primary key.
:medil_from_table: DB Table name of model in which the media is linked.
:medil_foreign_id: ID of the model in which the media is linked.
:medil_key: String key identifying the media linked.
:medil_media_id: Media Id of the media linked.

Relations
*********

.. php:attr:: media

	* Relation type : :term:`belongs_to`.
	* Model : :php:class:`Model_Media`
