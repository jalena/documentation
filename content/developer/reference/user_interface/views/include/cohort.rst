.. _reference/user_interface/views/cohort:

Cohort
======

.. raw:: html

   <span class="badge" style="background-color:#AD5E99">Enterprise feature</span>

The cohort view is used to display and understand the way some data changes over
a period of time.  For example, imagine that for a given business, clients can
subscribe to some service.  The cohort view can then display the total number
of subscriptions each month, and study the rate at which client leave the service
(churn). When clicking on a cell, the cohort view will redirect you to a new action
in which you will only see the records contained in the cell's time interval;
this action contains a list view and a form view.

.. note:: By default the cohort view will use the same list and form views as those
   defined on the action. You can pass a list view and a form view
   to the context of the action in order to set/override the views that will be
   used (the context keys to use being `form_view_id` and `list_view_id`)

For example, here is a very simple cohort view:

.. code-block:: xml

    <cohort string="Subscription" date_start="date_start" date_stop="date" interval="month"/>

The root element of the Cohort view is <cohort>, it accepts the following
attributes:

.. rst-class:: o-definition-list

``string`` (mandatory)
    A title, which should describe the view

``date_start`` (mandatory)
    A valid date or datetime field. This field is understood by the view as the
    beginning date of a record

``date_stop`` (mandatory)
    A valid date or datetime field. This field is understood by the view as the
    end date of a record.  This is the field that will determine the churn.

``mode`` (optional)
    A string to describe the mode. It should be either 'churn' or
    'retention' (default). Churn mode will start at 0% and accumulate over time
    whereas retention will start at 100% and decrease over time.

``timeline`` (optional)
    A string to describe the timeline. It should be either 'backward' or 'forward' (default).
    Forward timeline will display data from date_start to date_stop, whereas backward timeline
    will display data from date_stop to date_start (when the date_start is in future / greater
    than date_stop).

``interval`` (optional)
    A string to describe a time interval. It should be 'day', 'week', 'month''
    (default) or 'year'.

``measure`` (optional)
    A field that can be aggregated.  This field will be used to compute the values
    for each cell.  If not set, the cohort view will count the number of occurrences.

``<field>`` (optional)
  allows to specify a particular field in order to manage it from the available measures, it's
  main use is for hiding a field from the selectable measures:

  .. rst-class:: o-definition-list

  ``name`` (mandatory)
    the name of the field to use in the view.
  ``string`` (optional)
    the name that would be used to display the field in the cohort view, overrides the
    default python String attribute of the field.
  ``invisible`` (optional)
    if true, the field will not appear either in the active measures nor in the selectable
    measures (useful for fields that do not make sense aggregated, such as fields in different
    units, e.g. € and $).
    If the value is a domain, the domain is evaluated in the context of the current row's
    record, if ``True`` the corresponding attribute is set on the cell.
