Tools_Url
#########

.. php:namespace:: Nos

.. php:class:: Tools_Url

	Provide static methods for URL.

::page()
--------

.. php:staticmethod:: page($page_id)

	:params int $page_id: A :php:class:`Model_Page` ID.
	:returns: URL of the specify page.


::context()
-----------

.. php:staticmethod:: context($context)

	:params string $context: A context ID. See :doc:`/php/configuration/multi_context`.
	:returns: Home URL of the specify context.
