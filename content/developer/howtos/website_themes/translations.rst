============
Translations
============

In addition to creating great modern websites, Odoo gives you the possibility to translate it in
different languages.

In this chapter, you will learn how to:

- Translate the content of a module
- Import and Export translations
- Integrate translations to a module

Frontend
========

You can translate your website pages thanks to the website builder. Go to your site and switch to
the language you want to translate and then, click on the *Translate* button. Everything in green
are some translations that are done automatically. Everything in yellow are those that you can
translate yourself.

.. image:: translations/translate-button.png
      :alt: Translate button
      :width: 570

Backend
=======

You can also translate your pages directly from the backend. It will allow you to translate
different languages at the same time. Go to the views and find the page you want to translate and
then, click on the *Edit Translations* button.

.. image:: translations/edit-translations.png
      :alt: Edit translations
      :width: 718


Export
======

Once you have translated everything you wanted, you can export all your translations to integrate
them into your module. To export all in once, go in your database and go to:

:menuselection:`Settings --> Translations --> Import/Export --> Export translations`

**Export Settings**

- Language: the language you translated, e.g.: fr_BE, nl_BE
- File Format: .po
- Apps to export: website_airproof

Download the file and move it to the :file:`i18n` folder. You can always make changes directly in
the :file:`.po` file afterwards if you want.


PO file
=======

Translate directly with :file:`.po` file

.. code-block:: po
    :caption: ``/website_coconuts/i18n/fr_BE.po``

    #. module: website_airproof
    #: model_terms:ir.ui.view,arch_db:website_airproof.s_custom_snippet
    msgid "..."
    msgstr "..."

You can create the :file:`*.po` file yourself. You can use the :doc:`Odoo documentation <../translations>`
to write your translations.

Import
======

To import your translation files into Odoo, go to:

:menuselection:`Settings --> Translations --> Import/Export --> Import translations`
