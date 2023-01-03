==========
Going Live
==========

Once you've finished all the web design and development work, it's time to deploy it on a
development or on a production database.

Module Import
=============

Odoo SaaS
---------

For the first import:

#. Create a ZIP file of your module.
#. Connect to the project database.
#. Enable the *Developer Tools*.
#. Go to *Apps* and check that the module *Base import module* is installed.
#. Click on *Import Module* in the menu and upload your ZIP file.
#. Check the *Force Init* box.

In case you have made changes to your module and need to re-import it:

#. Create a ZIP file of your module.
#. Connect to the project database.
#. Enable the *Developer Tools*.
#. Click on the Debug mode icon and select *Become Superuser*.
#. Import your module.
#. Log out and log in again to deactivate the *Superuser* mode.

.. warning::
   The ZIP file size must be less than 50Mb.

Odoo.sh
-------

- Go to *Apps* and click on *Update Apps List* in the menu.
- Search for your module in the list and install it.

.. seealso::
   - :doc:`Introduction to Odoo.sh <../../../administration/odoo_sh/overview/introduction>`
