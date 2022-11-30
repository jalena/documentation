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

:menuselection:`Each model has Views that represent all its fields --> Backend & frontend views`

Models
~~~~~~

The basis of Odoo are the models. Models have fields in which records are stored. These records are
stored in a database (these records are therefore linked to a model). In Odoo, you can find the
different models in:

**Example:**

.. image:: setup/models-page.png
    :alt: Models page

Fields
~~~~~~

In a model, we will centralize fields (field names we need to target in our code).

**Classic fields**

- Date
- Char
- Selection
- …

**Relational fields**

These are fields that will call a field from another model. In other words, when we have linked a
record with another record (located on another template), we will be able to retrieve the content of
the fields that are located with this linked record. It allows you to link models together and make
them interact easily.

- **Many2one**: In a model, I have a field (= one) that I fill in by choosing from a list of records
  (= many) from another model. In Odoo, this field will always be named `"xxx_id"` whereas the
  many2many will have an "s": `"xxx_ids"`. For example, the "Customer" field on a quotation. We will
  have a list of several customers from which we must choose one.
- **One2many**: This is the reverse of Many2one. In a model, I have a field (= one) which lists the
  records (= many) of other tables. For example, the "Order Lines" tab on a quotation. We will see a
  list of several products ordered in one field.
- **Many2many**: It’s several records linked to several records (several can be selected). For
  example, the fact that you can put several tags on the same product and several products can have
  the same tag.

Views
~~~~~

Views are what define how records should be displayed to end-users. They are specified in XML which
means that they can be edited independently from the models that they represent. They are flexible
and allow a high level of customization of the screens that they control.

**Back-end vs Front-end**

- **Back-end view**: KanBan view, List view, Form view...
- **Front-end view**: Qweb view

**Static vs Dynamic**

- **Static views**: These are static content pages, like the homepage. For these pages, you can
  choose the url and set some properties like published, indexed, etc.
- **Dynamic views**: These are dynamically generated pages, such as the product page. Here the url
  is dynamic and is accessible to all by default. But there is a way to change this with access
  rights.

**Standard vs Inherited**

- **Standard views**: This is the "base" view, implemented by Odoo, which is directly derived from
  the model. You must not make any changes directly to this view. This allows us to update an Odoo
  database without overwriting the client's modifications (which will have been made on a duplicate
  view). Odoo only updates standard views.
- **Inherited views**: It’s a duplicated view. If there is a duplicate view, there will be two views
  with the same name in the database. But the duplicated view will not have an ID; there is an ID
  only for the standard view.

Dump
----

Please note that this part is optional. If you don’t need to import an existing database, you can
directly go to the next chapter: `Theme Module <https://docs.google.com/document/d/1AUDx1rdOyxecQ0Errf-AB7_OwevaiOxcYYhIHajct_Y/edit#heading=h.f0h9qbqq40pb>`_

**Odoo SaaS**

- Go to: `<database_url>/saas_worker/dump`

**Odoo.sh**

- Connect to Odoo.sh
- Select the branch you want to make a backup of
- Choose the *BACKUPS* tab.
- Click the *Create Backup* button.
- When the process is over, a notification will appear.
- Open it and click the *Go to Backup* button.
- Click on the *Download* icon button. Choose the *testing* option for *purpose* and *with filestore*
  for the *Filestore* option.

  .. image:: setup/download-backup.png
    :alt: Download backup

- You will receive a notification when the dump is ready to be downloaded. Open it and click on
  *Download* to get your dump.

  .. image:: setup/database-backup.png
    :alt: Database backup


Import
------

Getting Started
===============

Running Odoo
------------

Shell Script
------------

Sign In
-------

Developer Mode
--------------
