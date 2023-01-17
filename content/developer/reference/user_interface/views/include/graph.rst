.. _reference/user_interface/views/graph:

Graph
=====

The graph view is used to visualize aggregations over a number of records or
record groups. Its root element is ``<graph>`` which can take the following
attributes:

.. rst-class:: o-definition-list

``type`` (optional)
  one of ``bar`` (default), ``pie`` and ``line``, the type of graph to use

``stacked`` (optional)
  only used for ``bar`` charts. Set to ``0`` to prevent the bars within a group
  to be stacked initially.

``disable_linking`` (optional)
  set to ``1`` to prevent from redirecting clicks on graph to list view

``order`` (optional)
  if set, x-axis values will be sorted by default according their measure with
  respect to the given order (``asc`` or ``desc``). Only used for ``bar`` and
  ``pie`` charts.

``string`` (optional)
  string displayed in the breadcrumbs when redirecting to list view.

The only allowed element within a graph view is ``field`` which can have the
following attributes:

.. rst-class:: o-definition-list

``name`` (mandatory)
  the name of a field to use in the view. If used for grouping (rather
  than aggregating)

``invisible`` (optional)
  if true, the field will not appear either in the active measures nor in the
  selectable measures.

``type`` (optional)
  if set to ``measure``, the field will be used as an aggregated value within a
  group instead of a grouping criteria. It only works for the last field
  with that attribute but it is useful for other fields with string attribute
  (see below).

``interval`` (optional)
  on date and datetime fields, groups by the specified interval (``day``,
  ``week``, ``month``, ``quarter`` or ``year``) instead of grouping on the
  specific datetime (fixed second resolution) or date (fixed day resolution).
  Default is ``month``.

``string`` (optional)
  only used for field with ``type="measure"``. The name that will be used to
  display the field in the graph view, overrides the default python String
  attribute of the field.

The measures are automatically generated from the model fields; only the
aggregatable fields are used. Those measures are also alphabetically
sorted on the string of the field.

.. warning::

   graph view aggregations are performed on database content, non-stored
   function fields can not be used in graph views
