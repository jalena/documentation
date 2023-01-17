.. _reference/user_interface/views/qweb:

QWeb
====

QWeb views are standard :ref:`reference/qweb` templates inside a view's
``arch``. They don't have a specific root element. Because QWeb views don't
have a specific root element, their type must be specified explicitly (it can
not be inferred from the root element of the ``arch`` field).

QWeb views have two use cases:

* they can be used as frontend templates, in which case
  :ref:`reference/data/template` should be used as a shortcut.
* they can be used as actual qweb views (opened inside an action), in which
  case they should be defined as regular view with an explicit ``type`` (it
  can not be inferred) and a model.

The main additions of qweb-as-view to the basic qweb-as-template are:

* qweb-as-view has a special case for a ``<nav>`` element bearing the CSS
  class ``o_qweb_cp_buttons``: its contents should be buttons and will be
  extracted and moved to the control panel's button area, the ``<nav>`` itself
  will be removed, this is a work-around to control panel views not existing
  yet
* qweb-as-view rendering adds several items to the standard qweb rendering
  context:

  .. rst-class:: o-definition-list

  ``model``
    the model to which the qweb view is bound
  ``domain``
    the domain provided by the search view
  ``context``
    the context provided by the search view
  ``records``
    a lazy proxy to ``model.search(domain)``, this can be used if you just
    want to iterate the records and not perform more complex operations
    (e.g. grouping)
* qweb-as-view also provides additional rendering hooks:

  - ``_qweb_prepare_context(view_id, domain)`` prepares the rendering context
    specific to qweb-as-view
  - ``qweb_render_view(view_id, domain)`` is the method called by the client
    and will call the context-preparation methods and ultimately
    ``env['ir.qweb'].render()``.
