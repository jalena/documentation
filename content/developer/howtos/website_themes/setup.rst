=====
Setup
=====

In this chapter, you will learn how to:

- Setup your local development environment.
- Outline of Odoo Database structure.
- Export and import an Odoo database in your local environment.
- Have an Odoo instance up and running.

Install
=======

There are multiple ways to install Odoo, depending on the intended use case. This documentation
assumes you stick to the Source Install (running Odoo from the source code), which is best suited
for Odoo designers and developers.

.. seealso::
   :doc:`Installing Odoo <../../../administration/install/install>`

Databases
=========

Structure
---------

Every Odoo Application works in the same way; they are built in the same logic:

:menuselection:`A Model --> with Fields --> with Relational fields that link to other models`

:menuselection:`Each model has Views that represent all its fields --> Backend and frontend views`

Models
~~~~~~

The basis of Odoo are the models. Models have fields in which records are stored. These records are
stored in a database (these records are therefore linked to a model). In Odoo, you can find the
different models in the backend.

**Example:**

.. image:: setup/models-page.png
    :alt: Models page

.. note::
    To access models, you need to activate the :ref:`developer mode <developer-mode>`.

Fields
~~~~~~

In a model, we will centralize fields (field names we need to target in our code).

Classic fields
**************

- Date
- Char
- Selection
- …

Relational fields
*****************

These are fields that will call a field from another model. In other words, when we have linked a
record with another record (located on another template), we will be able to retrieve the content of
the fields that are located with this linked record. It allows you to link models together and make
them interact easily.

- **Many2one**: In a model, I have a field (= one) that I fill in by choosing from a list of records
  (= many) from another model. In Odoo, this field will always be named `"xxx_id"` whereas the
  many2many will have an "s": `"xxx_ids"`. For example, the *Customer* field on a quotation. We will
  have a list of several customers from which we must choose one.
- **One2many**: This is the reverse of Many2one. In a model, I have a field (= one) which lists the
  records (= many) of other tables. For example, the "Order Lines" tab on a quotation. We will see a
  list of several products ordered in one field.
- **Many2many**: It's several records linked to several records (several can be selected). For
  example, the fact that you can put several tags on the same product and several products can have
  the same tag.

Views
~~~~~

Views are what define how records should be displayed to end-users. They are specified in XML which
means that they can be edited independently from the models that they represent. They are flexible
and allow a high level of customization of the screens that they control.

Back-end vs Front-end
*********************

- **Back-end view**: KanBan view, List view, Form view...
- **Front-end view**: Qweb view

Static vs Dynamic
*****************

- **Static views**: These are static content pages, like the homepage. For these pages, you can
  choose the url and set some properties like published, indexed, etc.
- **Dynamic views**: These are dynamically generated pages, such as the product page. Here the url
  is dynamic and is accessible to all by default. But there is a way to change this with access
  rights.

Standard vs Inherited
*********************

- **Standard views**: This is the "base" view, implemented by Odoo, which is directly derived from
  the model. You must not make any changes directly to this view. This allows us to update an Odoo
  database without overwriting the client's modifications (which will have been made in an inherited
  view). Odoo only updates standard views.
- **Inherited views**: It's a duplicated view. If there is a duplicate view, there will be two views
  with the same name in the database. But the duplicated view will not have an ID; there is only an 
  ID for the standard view.

Dump
----

Please note that this part is optional. If you don't need to import an existing database, you can
directly go to the next chapter: :ref:`Theming <theming>`

Odoo SaaS
~~~~~~~~~

- Go to: `<database_url>/saas_worker/dump`

Odoo.sh
~~~~~~~

#. Connect to Odoo.sh
#. Select the branch you want to make a backup of
#. Choose the *BACKUPS* tab.
#. Click the *Create Backup* button.
#. When the process is over, a notification will appear.
#. Open it and click the *Go to Backup* button.
#. Click on the *Download* icon button. Choose the *testing* option for *purpose* and *with filestore*
   for the *Filestore* option.

   .. image:: setup/download-backup.png
     :alt: Download backup

#. You will receive a notification when the dump is ready to be downloaded. Open it and click on
   *Download* to get your dump.

   .. image:: setup/database-backup.png
     :alt: Database backup

Import
------

Please note that this part is optional. If you don't need to import an existing database, you can
directly go to the next chapter: :doc:`Theme Module <theming>`

Move Filestore
~~~~~~~~~~~~~~

Copy/paste all the folders included in the filestore folder to the right location on your computer:

- macOS: `/Users/<User>/Library/Application Support/Odoo/filestore/<database_name>`
- Linux: `/home/<User>/.local/share/Odoo/filestore/<database_name>`

.. note::
   `/Library` is a hidden folder.

Database setup
~~~~~~~~~~~~~~

Create an empty database:

.. code-block:: xml

    createdb <database_name>

Import the SQL file in the database that you just created:

.. code-block:: xml

    psql <database_name> < dump.sql

Reset admin user password:

.. code-block:: xml

    psql \c <database_name>
    update res_users set login='admin', password='admin' where id=2;

Getting Started
===============

Running Odoo
------------

Once all dependencies are set up, Odoo can be launched by running `odoo-bin`, the command-line
interface of the server. It is located at the root of the Odoo Community directory.

- :ref:`Windows <setup/install/source/linux/running_odoo>`
- :ref:`Linux <setup/install/source/linux/running_odoo>`
- :ref:`Mac OS <setup/install/source/linux/running_odoo>`
- `Docker <https://hub.docker.com/_/odoo/>`_

To configure the server, you can specify command-line arguments or a configuration file. For this
documentation, we're going to use the first method.

The CLI offers several functionalities related to Odoo. You can use it to run the server, scaffold
an Odoo Theme, populate a database, or count the number of lines of code.

Shell Script
------------

A typical way to run the server would be to add all the command line arguments to a `.sh` script.

**Example:**

.. code-block:: xml

    ./odoo-bin --addons-path=../enterprise,addons --db-filter=<database> -d <database> --without-demo=all -i website --dev=xml

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Folder
     - Description
   * - --addons-path
     - Comma-separated list of directories in which modules are stored. These directories are
       scanned for modules.
   * - -d

       --database
     - database(s) used when installing or updating modules.
   * - --db-filter
     - Hides databases that do not match the filter.
   * - -i

       --init
     - Comma-separated list of modules to install before running the server. (requires `-d`)
   * - -u

       --update
     - Comma-separated list of modules to update before running the server. (requires `-d`)
   * - --without-demo
     - Disable demo data loading for modules installed comma-separated, use all for all modules.
       (requires `-d` and `-i`)
   * - --dev
     - Comma-separated list of features. For development purposes only. :ref:`More info <reference/cmdline/dev>`

.. seealso::
   :ref:`Command-line Arguments <reference/cmdline/server>`

Sign In
-------

After the server has started (the INFO log `odoo.modules.loading: Modules loaded.` is printed), open
`http://localhost:8069` in your web browser, and log in with the base administrator account.

Use **admin** for the Email and, again, **admin** for the Password. That's it! You just logged into your own
Odoo database!

.. image:: setup/welcome-homepage.png
    :alt: Welcome homepage

.. tip::
   Hit *CTRL+C* to stop the server. Do it twice if needed.

Developer Mode
--------------

The developer mode also known as debug mode is useful for development as it gives access to
additional tools. In the next chapters, we will always assume that you have enabled the developer
mode.

.. seealso::
   :ref:`How to enable the developer mode <developer-mode>`
