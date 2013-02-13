Virtualpath
###########

.. php:namespace:: Nos

.. php:class:: Orm_Behaviour_Virtualpath

	| Extend :php:class:`Orm_Behaviour_Virtualname`.
	| Add a virtual path property to item.

.. seealso::

	:php:class:`Orm_Behaviour_Virtualname` for configuration and methods.

Configuration
*************

.. php:attr:: virtual_path_property

	Required.
	Column name use for save virtual path.

.. php:attr:: extension_property

	Required.
	String to add to the end of the virtual path.
	If is ``array``:

		:before: String to add to extension start.
		:after: String to add to extension end.
		:property: Column name to use for extension.

.. php:attr:: extension_property

	Required.
	Name of the parent :term:`relation <Relations>` use for generate first part of virtual path.

Methods
*******

.. php:method:: virtual_path($dir = false)

	:params boolean $dir: If ``true``, extension replace by a final ``/``.
	:returns: Virtual path of item.

.. php:method:: extension()

	:returns: Return extension part of the virtual path.

Example
*******

.. code-block:: php

	<?php
	class Model_Page extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Virtualpath' => array(
				'events' => array('before_save', 'after_save', 'change_parent'),
				'virtual_name_property' => 'page_virtual_name',
				'virtual_path_property' => 'page_virtual_url',
				'extension_property' => '.html',
				'parent_relation' => 'parent',
			),
		);
	}
