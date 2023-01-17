.. _reference/user_interface/views/activity:

Activity
========

The Activity view is used to display the activities linked to the records. The
data are displayed in a chart with the records forming the rows and the activity
types the columns. The first cell of each row displays a (customizable, see
``templates``, quite similarly to :ref:`reference/user_interface/views/kanban`) card representing
the corresponding record. When clicking on others cells, a detailed description
of all activities of the same type for the record is displayed.

.. warning::

   The Activity view is only available when the ``mail`` module is installed,
   and for the models that inherit from the ``mail.activity.mixin``.

The root element of the Activity view is ``<activity>``, it accepts the following
attributes:

.. rst-class:: o-definition-list

``string`` (mandatory)
    A title, which should describe the view

Possible children of the view element are:

.. rst-class:: o-definition-list

``field``
  declares fields to use in activity *logic*. If the field is simply displayed
  in the activity view, it does not need to be pre-declared.

  Possible attributes are:

  .. rst-class:: o-definition-list

  ``name`` (required)
    the name of the field to fetch

``templates``
  defines the :ref:`reference/qweb` templates. Cards definition may be
  split into multiple templates for clarity, but activity views *must* define at
  least one root template ``activity-box``, which will be rendered once for each
  record.

  The activity view uses mostly-standard :ref:`javascript qweb
  <reference/qweb/javascript>` and provides the following context variables
  (see :ref:`reference/user_interface/views/kanban` for more details):

  .. rst-class:: o-definition-list

  ``widget``
    the current :js:class:`ActivityRecord`, can be used to fetch some
    meta-information. These methods are also available directly in the
    template context and don't need to be accessed via ``widget``
  ``record``
    an object with all the requested fields as its attributes. Each field has
    two attributes ``value`` and ``raw_value``
