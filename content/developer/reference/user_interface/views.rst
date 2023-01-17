.. _user_interface/views:

=====
Views
=====

.. _user_interface/views/options:

Generic view options/attributes
===============================

The different view types have a wide variety of attributes allowing customizations of
the generic behaviors. Some main attributes will be explained here. They do not all have
an impact on all view types.

.. todo:: info on create/...

:sample:
  boolean_ (optional)

  (``kanban`` & ``list`` & ``gantt`` & ``graph`` & ``pivot`` & ``cohort``)

  Populate the view with a set of sample records if none are found for the current model.
  This attribute is false by default.

  These fake records will have heuristics for certain field names/models. For example,
  a field 'display_name' on the model 'res.users' will be populated with sample people names
  while an 'email' field will be in the form 'firstname.lastname@sample.demo'.

  The user will not be able to interact with these data and they will be discarded as soon as
  an action is performed (record created, column added, etc.)

:banner_route:
  path_ (optional)

  a route address to be fetched and prepended to the view.

  If this attribute is set, the
  :ref:`controller route url<reference/controllers>` will be fetched and
  displayed above the view. The json response from the controller should
  contain an "html" key.

  If the html contains a stylesheet <link> tag, it will be
  removed and appended to <head>.

  To interact with the backend you can use <a type="action"> tags. Please take
  a look at the documentation of the _onActionClicked method of
  AbstractController (*addons/web/static/src/js/views/abstract_controller.js*)
  for more details.

  Only views extending AbstractView and AbstractController can use this
  attribute, like :ref:`reference/user_interface/views/form`, :ref:`reference/user_interface/views/kanban`,
  :ref:`reference/user_interface/views/list`, ...

  Example:

  .. code-block:: xml

      <tree banner_route="/module_name/hello" />

  .. code-block:: python

      class MyController(odoo.http.Controller):
          @http.route('/module_name/hello', auth='user', type='json')
          def hello(self):
              return {
                  'html': """
                      <div>
                          <link href="/module_name/static/src/css/banner.css"
                              rel="stylesheet">
                          <h1>hello, world</h1>
                      </div> """
              }

.. note:: The current context and user access rights may also impact the view abilities.

.. _user_interface/views/python_expression:

.. note::
    In the views architecture, some XML node attributes canuse `python expression`_,
    in this case this expression is evaluated with the field values of the current view
    and this other values:

    * ``context``: dict_ with the values of the current view context;
    * ``uid``: integer_ the id of the current user;
    * ``today``: string_ the current local date as a string of the form ``YYYY-MM-DD``;
    * ``now``: string_ same as ``today`` with the addition of the current time. This value is formatted as ``YYYY-MM-DD hh:mm:ss``;
    * ``parent``: The properties of this "object" refere to the container view values. This magic object is only available for the subviews of :ref:`relational fields <studio/fields/relational-fields>`.

.. include:: views/include/form.rst
.. include:: views/include/settings.rst
.. include:: views/include/list.rst
.. include:: views/include/search.rst
.. include:: views/include/kanban.rst
.. include:: views/include/qweb.rst
.. include:: views/include/graph.rst
.. include:: views/include/pivot.rst
.. include:: views/include/calendar.rst
.. include:: views/include/activity.rst
.. include:: views/include/cohort.rst
.. include:: views/include/grid.rst
.. include:: views/include/gantt.rst
.. include:: views/include/map.rst

.. _`python expression`: https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not
.. _dict: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
.. _integer: https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex
.. _string: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str
