.. _reference/user_interface/views/search:

Search
======

Search views are a break from previous view types in that they don't display
*content*: although they apply to a specific model, they are used to filter
other view's content (generally aggregated views
e.g. :ref:`reference/user_interface/views/list` or :ref:`reference/user_interface/views/graph`). Beyond that
difference in use case, they are defined the same way.

The root element of search views is ``<search>``. It takes no attributes.

.. @string is not displayed anywhere, should be removed

Possible children elements of the search view are:

.. rst-class:: o-definition-list

``field``
    fields define domains or contexts with user-provided values. When search
    domains are generated, field domains are composed with one another and
    with filters using **AND**.

    Fields can have the following attributes:

    .. rst-class:: o-definition-list

    ``name``
        the name of the field to filter on
    ``string``
        the field's label
    ``operator``
        by default, fields generate domains of the form :samp:`[({name},
        {operator}, {provided_value})]` where ``name`` is the field's name and
        ``provided_value`` is the value provided by the user, possibly
        filtered or transformed (e.g. a user is expected to provide the
        *label* of a selection field's value, not the value itself).

        The ``operator`` attribute allows overriding the default operator,
        which depends on the field's type (e.g. ``=`` for float fields but
        ``ilike`` for char fields)
    ``filter_domain``
        complete domain to use as the field's search domain, can use a
        ``self`` variable to inject the provided value in the custom
        domain. Can be used to generate significantly more flexible domains
        than ``operator`` alone (e.g. searches on multiple fields at once)

        If both ``operator`` and ``filter_domain`` are provided,
        ``filter_domain`` takes precedence.
    ``context``
        allows adding context keys, including the user-provided values (which
        as for ``domain`` are available as a ``self`` variable, an array of
        values e.g. ``[id_1, id_2]`` for a :class:`~odoo.fields.Many2one` field).
        By default, fields don't generate domains.

        .. note:: the domain and context are inclusive and both are generated
                  if a ``context`` is specified. To only generate context
                  values, set ``filter_domain`` to an empty list:
                  ``filter_domain="[]"``
    ``groups``
        make the field only available to specific users
    ``domain``
        if the field can provide an auto-completion
        (e.g. :class:`~odoo.fields.Many2one`), filters the possible
        completion results.

``filter``
    a filter is a predefined toggle in the search view, it can only be enabled
    or disabled. Its main purposes are to add data to the search context (the
    context passed to the data view for searching/filtering), or to append new
    sections to the search filter.

    Filters can have the following attributes:

    .. rst-class:: o-definition-list

    ``string`` (required)
        the label of the filter
    ``domain`` (optional)
        an Odoo :ref:`domain <reference/orm/domains>`, will be appended to the
        action's domain as part of the search domain.
    ``date`` (optional)
        the name of a field of type ``date`` or ``datetime``.
        Using this attribute has the effect to create
        a set of filters available in a submenu
        of the filters menu. The filters proposed are time dependent
        but not dynamic in the sense that their domains are evaluated
        at the time of the control panel instantiation.

        Example:

        .. code-block:: xml

          <filter name="filter_create_date" date="create_date" string="Creation Date"/>

        The example above allows to easily search for records with creation date field
        values in one of the periods below (if the current month is August 2019).

        .. code-block:: text

          Create Date >
            August
            July
            June
            Q4
            Q3
            Q2
            Q1
          --------------
            2019
            2018
            2017

        Multi selection of options is allowed.

    ``default_period`` (optional)
        only makes sense for a filter with non empty ``date`` attribute.
        determines which periods are activated if the filter is in the
        default set of filters activated at the view initialization. If not provided,
        'this_month' is used by default.

        To choose among the following options:
        today, this_week, this_month, last_month, antepenultimate_month,
        fourth_quarter, third_quarter, second_quarter, first_quarter,
        this_year, last_year, antepenultimate_year.

        The attribute accepts comma separated values.

        Examples:

        .. code-block:: xml

          <filter name="filter_create_date" date="create_date" string="Creation Date" default_period="this_week"/>
          <filter name="filter_create_date" date="create_date" string="Creation Date" default_period="this_year,last_year"/>

    ``context``
        a Python dictionary, merged into the action's domain to generate the
        search domain

        The key ``group_by`` can be used to define a groupby available in the
        'Group By' menu. The 'group_by' value can be a valid field name.

        .. code-block:: xml

           <filter name="groupby_category" string="Category" context="{'group_by': 'category_id'}"/>

        The groupby defined above allows to group data by category.

        When the field is of type ``date`` or ``datetime``, the filter generates a submenu of the Group By
        menu in which the following interval options are available: day, week, month, quarter, year.

        In case the filter is in the default set of filters activated at the view initialization,
        the records are grouped by month by default. This can be changed by using the syntax
        'date_field:interval' as in the following example.

        Example:

        .. code-block:: xml

           <filter name="groupby_create_date" string="Creation Date" context="{'group_by': 'create_date:week'}"/>

        .. note::
           The results of read_groups grouped on a field may be influenced by its group_expand attribute,
           allowing to display empty groups when needed.  For more information, please refer to
           :class:`~odoo.fields.Field` attributes documentation.

    ``name``
        logical name for the filter, can be used to :ref:`enable it by default
        <reference/user_interface/views/search/defaults>`, can also be used as
        :ref:`inheritance hook <reference/view_record/inheritance>`
    ``help``
        a longer explanatory text for the filter, may be displayed as a
        tooltip
    ``groups``
        makes a filter only available to specific users

    .. tip::

       .. versionadded:: 7.0

       Sequences of filters (without non-filters separating them) are treated
       as inclusively composited: they will be composed with ``OR`` rather
       than the usual ``AND``, e.g.

       ::

          <filter domain="[('state', '=', 'draft')]"/>
          <filter domain="[('state', '=', 'done')]"/>

       if both filters are selected, will select the records whose ``state``
       is ``draft`` or ``done``, but

       ::

          <filter domain="[('state', '=', 'draft')]"/>
          <separator/>
          <filter domain="[('delay', '<', 15)]"/>

       if both filters are selected, will select the records whose ``state``
       is ``draft`` **and** ``delay`` is below 15.

``separator``
    can be used to separates groups of filters in simple search views

``group``
    can be used to separate groups of filters, more readable than
    ``separator`` in complex search views

``searchpanel``
  allows to display a search panel on the left of any multi records view.
  By default, the list and kanban views have the searchpanel enabled.
  The search panel can be activated on other views with the attribute:

  .. rst-class:: o-definition-list

  ``view_types``
    a comma separated list of view types on which to enable the search panel
    default: 'tree,kanban'

  This tool allows to quickly filter data on the basis of given fields. The fields
  are specified as direct children of the ``searchpanel`` with tag name ``field``,
  and the following attributes:

  .. rst-class:: o-definition-list

  ``name`` (mandatory)
    the name of the field to filter on

  ``select``
    determines the behavior and display. Possible values are

    * ``one`` (default) at most one value can be selected. Supported field types are
      many2one and selection.

    * ``multi`` several values can be selected (checkboxes). Supported field
      types are many2one, many2many and selection.

    ``groups``
      restricts to specific users

    ``string``
      determines the label to display

    ``icon``
      specifies which icon is used

    ``color``
      determines the icon color

    Additional optional attributes are available in the ``multi`` case:

    .. rst-class:: o-definition-list

    ``enable_counters``
      default is false. If set to true the record counters will be computed and
      displayed if non-zero.

      This feature has been implemented in case performances would be too bad.

      Another way to solve performance issues is to properly override the
      ``search_panel_select_range`` and ``search_panel_select_multi_range`` methods.

    ``expand``
      default is false. If set to false categories or filters with 0 records will be hidden.

    ``limit``
      default is 200. Integer determining the maximal number of values to fetch for the field.
      If the limit is reached, no values will be displayed in the search panel and an error message will
      appear instead because we consider that is useless / bad performance-wise. All values will be
      fetched if set to 0.

    Additional optional attributes are available according to the chosen case:

    - For the ``one`` case:

      .. rst-class:: o-definition-list

      ``hierarchize``
        (only available for many2one fields) default is true. Handles the display style of categories :

        If set to true child categories will appear under their related parent.
        If not, all categories will be displayed on the same level.

    - For the ``multi`` case:

      .. rst-class:: o-definition-list

      ``domain``:
        determines conditions that the comodel records have to satisfy.

  A domain might be used to express a dependency on another field (with select="one")
  of the search panel. Consider
  /!\ This attribute is incompatible with a select="one" with enabled counters; if a select="multi"
  has a `domain` attribute, all select="one" will have their counters disabled.

  .. code-block:: xml

    <searchpanel>
      <field name="department_id"/>
      <field name="manager_id" select="multi" domain="[('department_id', '=', department_id)]"/>
    </searchpanel>

  In the above example, the range of values for manager_id (manager names) available at screen
  will depend on the value currently selected for the field ``department_id``.

  * ``groupby``: field name of the comodel (only available for many2one and many2many fields). Values will be grouped by that field.

.. _reference/user_interface/views/search/defaults:

Search defaults
---------------

Search fields and filters can be configured through the action's ``context``
using :samp:`search_default_{name}` keys. For fields, the value should be the
value to set in the field, for filters it's a boolean value or a number. For instance,
assuming ``foo`` is a field and ``bar`` is a filter an action context of:

.. code-block:: python

  {
    'search_default_foo': 'acro',
    'search_default_bar': 1
  }

will automatically enable the ``bar`` filter and search the ``foo`` field for
*acro*.

A numeric value (between 1 and 99) can be used to describe the order of default groupbys.
For instance if ``foo`` and ``bar`` refer to two groupbys

.. code-block:: python

  {
    'search_default_foo': 2,
    'search_default_bar': 1
  }

has the effect to activate first ``bar`` then ``foo``.
