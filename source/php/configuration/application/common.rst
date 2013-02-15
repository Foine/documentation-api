Common
######

Configuration for :php:class:`Model`, used in :doc:`appdesk`, :doc:`crud` or :doc:`inspector`.

Associative array:

:data_mapping: columns on :doc:`appdesk` and :doc:`inspector`.
:i18n: Optional, common translation
:actions: Optional, common actions on the :php:class:`Model`.
:icons: Optional, common icon related to the :php:class:`Model`.

Data mapping
************

Associative array where each key => value defines a column, all keys are optionals.

:title: Title of the grid column. If not set, column will not be displayed.
:column: Default value is same as key.
:search_column: Default value is column key value. Defines where on which sql column search / order.
:search_relation: Default value is deduced from key (ex: "rel->col"). Relation to load (via related function on query).
:multiContextHide: Hide grid column when items are filtered on more than one contexts.
:value: A closure function taking current item :php:class:`Model` in first parameter. Overloads value displayed in grid.
:cellFormatters: Associative array of :ref:`cellFormatters <php/configuration/application/cellFormatters>` for formatting column display.

.. code-block:: php

    <?php
    return array(
        'data_mapping' => array(
            'column_a' => array(
                'title' => 'Column A',
                'column' => 'col_a',
                'multiContextHide' => true,
                'cellFormatters' => array(
                    'center' => array(
                        'type' => 'css',
                        'css' => array('text-align' => 'center'),
                    ),
                ),
            ),
            'column_b' => array(
                'title' => 'B',
                'search_column' => 'col_b_search',
                'search_relation' => 'rel',
                'value' => function($item) {
                    return 'test';
                },
            ),
            // ...
        ),
    );

Particular cases
================

In next example, ``column_a`` is sent in json but will not be displayed.

.. code-block:: php

    <?php
    return array(
        'data_mapping' => array(
            'column_a',
        ),
    );

In next example, ``col_b`` is sent in json under the column_b key but will not be displayed.

.. code-block:: php

    <?php
    return array(
        'data_mapping' => array(
            'column_b' => 'col_b',
        ),
    );


If the :php:class:`Model` have behaviour :php:class:`Orm_Behaviour_Twinnable`, a pseudo column ``context`` is automatically add at the end of ``data_mapping``.
But, if you want to position elsewhere, you can refrence:

.. code-block:: php

    <?php
    return array(
        'data_mapping' => array(
            'column_a' => array(
                'title' => 'Column a'
            ),
            'context',
            'column_b' => array(
                'title' => 'Column b'
            ),
        ),
        // ...
    );

I18n
****

This key contains all common translations.

.. code-block:: php

    <?php
    return array(
        'i18n' => array(
            // Crud
            'notification item added' => __('Done! The item has been added.'),
            'notification item saved' => __('OK, all changes are saved.'),
            'notification item deleted' => __('The item has been deleted.'),

            // General errors
            'notification item does not exist anymore' => __('This item doesn’t exist any more. It has been deleted.'),
            'notification item not found' => __('We cannot find this item.'),

            // ... extends /framework/config/i18n_common.config.php
        ),
    );

.. _php/configuration/application/common/actions:

Actions
*******

This key contains all common actions related to the model. There are 5 actions automatically added:

* ``add``: the "Add model" button located at the appdesk's toolbar
* ``edit``: The "Edit" button located at the grids and crud toolbar
* ``delete``: The "Edit" button located at the grids and crud toolbar
* ``visualize``:
* ``share``:

The action key can be filled in two different ways.

The most common way is to define an associative array:

.. code-block:: php

    <?php
    return array(
        // ...
        'actions' => array(
            'action_1' => array(/* configuration */),
            'action_2' => array(/* configuration */),
            // ...
        ),
    );

If you want to define the order in which the actions are defined, two keys are to be defined:

:list: associative array of actions (similar to previous 'actions' key
:order: array of action key defining their order

.. code-block:: php

    <?php
    return array(
        // ...
        'actions' => array(
            'list' => array(
                'action_1' => array(/* configuration */),
                'action_2' => array(/* configuration */),
                // ...
            ),
            'order' => array(
                'action_2',
                'action_1'
            ),
        ),
    );

Each action is defined by a key => value. Key is the action id, and value is an array defining the action configuration:

:action: defines the action executed when action is triggered (using :doc:`/javascript/$/nosAction`)
:label: Text associated to action (displayed or on tooltip)
:primary: Is the action a primary action. On the grid,
:icon: Icon of the action. The string is appended to "ui-icon-" in order to obtain `jquery ui icon class <http://jqueryui.com/themeroller/#icons`
:red: Is the action red or not
:targets: Where to display the action. It is an associated array where keys defines where to display the action,
the value a boolean defining whether or not the action is displayed. ``targets`` can refined by the ``visible`` key There are 3 available keys :

    :grid: Is the action displayed on the grid (appdesk and inspector)
    :toolbar-grid: Is the action displayed on the grid toolbar
    :toolbar-edit: Is the action displayed on the crud edit toolbar

:enabled: Callback function

.. code-block:: php

    <?php
    return array(
        'actions '=> array(
            'action_id' => array(
                'action' => array(
                    'action' => 'confirmationDialog',
                    'dialog' => array(
                        'contentUrl' => '{{controller_base_url}}delete/{{_id}}',
                        'title' => 'Delete',
                    ),
                ),
                'label' => __('Delete'),
                'primary' => true,
                'icon' => 'trash',
                'red' => true,
                'targets' => array(
                    'grid' => true,
                    'toolbar-edit' => true,
                ),
                'enabled' => function($item) {
                    return true;
                },
                'visible' => function($params) {
                    return !isset($params['item']) || !$params['item']->is_new();
                },
            ),
        ),
    );

Placeholders
============

Particular cases
================

Icons
*****

This key contains all common icons related to the model. Structure is similar to the icons key in :doc:`metadata` configuration file :

.. code-block:: php

    <?php
    return array(
        'icons' => array(
            64 => 'static/apps/noviusos_page/img/64/page.png',
            32 => 'static/apps/noviusos_page/img/32/page.png',
            16 => 'static/apps/noviusos_page/img/16/page.png',
        ),
    );
