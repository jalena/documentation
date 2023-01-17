.. _reference/user_interface/views/pivot:

Pivot
=====

The pivot view is used to visualize aggregations as a `pivot table`_. Its root
element is ``<pivot>`` which can take the following attributes:

.. rst-class:: o-definition-list

``disable_linking`` (optional)
  Set to ``1`` to remove table cell's links to list view.
``display_quantity`` (optional)
  Set to ``1`` to display the Quantity column by default.
``default_order`` (optional)
  The name of the measure and the order (asc or desc) to use as default order
  in the view.

  .. code-block:: xml

     <pivot default_order="foo asc">
        <field name="foo" type="measure"/>
     </pivot>

The only allowed element within a pivot view is ``field`` which can have the
following attributes:

.. rst-class:: o-definition-list

``name`` (mandatory)
  the name of a field to use in the view. If used for grouping (rather
  than aggregating)

``string`` (optional)
  the name that will be used to display the field in the pivot view,
  overrides the default python String attribute of the field.

``type`` (optional)
  indicates whether the field should be used as a grouping criteria or as an
  aggregated value within a group. Possible values are:

  .. rst-class:: o-definition-list

  ``row`` (default)
    groups by the specified field, each group gets its own row.
  ``col``
    creates column-wise groups
  ``measure``
    field to aggregate within a group
  ``interval``
    on date and datetime fields, groups by the specified interval (``day``,
    ``week``, ``month``, ``quarter`` or ``year``) instead of grouping on the
    specific datetime (fixed second resolution) or date (fixed day resolution).

``invisible`` (optional)
  if true, the field will not appear either in the active measures nor
  in the selectable measures (useful for fields that do not make sense aggregated,
  such as fields in different units, e.g. € and $).

The measures are automatically generated from the model fields; only the
aggregatable fields are used. Those measures are also alphabetically
sorted on the string of the field.

.. warning::

    like the graph view, the pivot aggregates data on database content
    which means that non-stored function fields can not be used in pivot views


In Pivot view a ``field`` can have a ``widget`` attribute to dictate its format.
The widget should be a field formatter, of which the most interesting are
``date``, ``datetime``, ``float_time``, and ``monetary``.

For instance a timesheet pivot view could be defined as::

    <pivot string="Timesheet">
        <field name="employee_id" type="row"/>
        <field name="date" interval="month" type="col"/>
        <field name="unit_amount" type="measure" widget="float_time"/>
    </pivot>


.. _pivot table: https://en.wikipedia.org/wiki/Pivot_table
