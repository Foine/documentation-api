Events
######

The back-office of Novius OS is one "big HTML page". Actions performed in one tab can affect other tabs (ex: adding, modifying or deleting an item).

An event system has been established to enable the various interface elements to communicate with each other.

| On the one side, the interface elements are listening to events (by binding callbacks functions) by connecting to :term:`dispatchers <dispatcher>`.
| On the other side, the different actions trigger events, usually returned by AJAX requests (see :js:func:`$container.nosAjax`),
  which are then dispatched to all interface elements via :term:`dispatchers <dispatcher>`.

Dispatched events are executed immediately on the current active tab or popup (the one which has focus).
For other tabs (or popups), they are executed only when the tab or popup becomes active / focused.

.. glossary::

	dispatcher
		| A DOM element that receives system events. The child elements of the dispatcher can listen for events by connecting to it.
		| Each tab and popup have a dispatcher.

Structure of an event
*********************

.. js:data:: Event

.. js:attribute:: Event.name

	| ``string``
	| Required
	| Event name. For events on a ``Model``, the name is the Model name, including its PHP namespace.

.. js:attribute:: Event.id

	| ``int`` or ``[int]``
	| For events on a ``Model``, ID(s) of the item to which they relate.

.. js:attribute:: Event.action

	| ``string``
	| Name of the action item that triggered the event. Ex: ``insert``, ``update`` or ``delete``.

.. js:attribute:: Event.context

	| ``string`` or ``[string]``
	| Context of the item that triggered the event. See :doc:`/php/configuration/software/multi_context`.


nosListenEvent
**************

.. js:function:: $container.nosListenEvent(event, callback [, caller ])

	| Listen one (or many) event(s), i.e. register a callback function to be called when the event occurs.
	| Listening will be on current :term:`dispatcher` (closest relatives in the DOM element in jQuery container).

	For the callback function to be triggered, listened and triggered events should not match exactly.
	The listened event can just match one property of the triggered event.

	:param mixed event: ``{}`` or ``[{}]``. Required. JSON event to listen.
	:param function callback: Required. The callback function to execute when the event occurs. The function takes as parameter the triggered event.
	:param string caller: Caller name. If set, can stop listening to specific listener. See :js:func:`$container.nosUnlistenEvent`.

	.. code-block:: js

		// Listen all events with name 'Nos\Model_Page'
		$(domContext).nosListenEvent({
			name: 'Nos\Model_Page'
		}, function(event) {
			// ...
		}, 'caller');

		// Listen all events with the 'Nos\Model_Page' name and 'insert' or 'delete' actions
		$(domContext).nosListenEvent({
				name: 'Nos\Model_Page',
				action: ['insert', 'delete']
			},
			function(event) {
				// ...
			});

		// Listen all events with the 'Nos\Model_Page' name and 'insert' or 'delete' actions,
		// or events with the 'Nos\Model_Page' name and the 'main::en_GB' context
		$(domContext).nosListenEvent([
			{
				name: 'Nos\Model_Page',
				action: ['insert', 'delete']
			},
			{
				name; 'Nos\Model_Page',
				context; 'main::en_GB'
			}
		], function(event) {
			// ...
		});

nosUnlistenEvent
****************

.. js:function:: $container.nosUnlistenEvent(caller)

	Stop listening events for a specific caller. See :js:func:`caller param of nosListenEvent <$container.nosListenEvent>`.

	:param string caller: Caller name.

	.. code-block:: js

		$(domContext).nosUnlistenEvent('caller');

nosDispatchEvent
****************

.. js:function:: $.nosDispatchEvent(event)

	Dispatches an event to all available :term:`dispatchers <dispatcher>`.

	:param JSON event: See :js:data:`Event`.

	.. code-block:: js

		// Dispatch event, page with ID 4 has been create with 'main::en_GB' context
		$.nosDispatchEvent({
			name: 'Nos\Model_Page',
			action: 'insert',
			id: 4,
			context: 'main::en_GB',
		});

