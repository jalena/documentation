=======
Theming
=======

After your development environment is fully set up, let’s start building the skeleton of your theme
module. In this chapter, you will also discover how to:

- Enable/disable Website Builder standard options and templates
- Define the colors and the fonts to use for your design
- Get the most of Bootstrap variables
- Add custom styles and javascript

Theme Module
============

Odoo comes with a default `theme` that provides minimal structure and layout.
When you create a new theme, you are actually extending it.

Don’t forget to add the directory containing your module to the addons-path command-line argument
when running Odoo in your development environment.

Technical Naming
----------------

The very first step is to create a new directory.

.. code-block:: xml

   website_airproof

.. note::
   Prefix with `website_` and use only lowercase and underscores

Structure
---------

Odoo’s themes are packaged like any modules. Even if you are designing a very simple website for
your company or client, you need to package the theme like an Odoo module.

.. code-block:: xml

   /website_airproof
    ↳ /data
    ↳ /i18n
    ↳ /lib
    ↳ /static
    ↳ /views
    __init__.py
    __manifest__.py

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Folder
     - Description
   * - Data
     - Presets, menus, pages, images, shapes… (*.xml)
   * - i18n
     - Translations (*.po, *.pot)
   * - lib
     - External libraries (*.js)
   * - static
     - Custom assets (*.jpg, *.gif, *.png, *.svg, *.pdf, *.scss, *.js)
   * - views
     - Custom views and templates (*.xml)

Initialization
--------------

An Odoo module is also a Python package with a `__init__.py` file, containing import instructions for
various Python files in the module.
This file can remain empty for now.

Declaration
-----------

An Odoo module is declared by its manifest. This file declares a python package as an Odoo module
and specifies module metadata. This file must describe our module and cannot remain empty. Its only
required field is the name, but it usually contains much more information.

.. code-block:: python
    :caption: ``/website_airproof/__manifest__.py``

    {
       'name': 'Airproof Theme',
       'description': '...',
       'category': 'Website/Theme',
       'version': '15.0.0',
       'author': '...',
       'license': '...',
       'depends': ['website'],
       'data': [
	      # ...
       ],
       'assets': {
	      # ...
       },
    }

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Field
     - Description
   * - name
     - Human-readable name of the module (required)
   * - description
     - Extended description for the module, in reStructuredText
   * - category
     - Classification category within Odoo
   * - version
     - Odoo version this module is addressing
   * - author
     - Name of the module author
   * - license
     - Distribution license for the module
   * - depends
     - Odoo modules which must be loaded before this one, either because this module uses features
       they create or because it alters resources they define
   * - data
     - List of XML files
   * - assets
     - List of SCSS and JS files

.. note::
   To create a website theme, you only need to install the Odoo Website app.
   If you need other apps  (Blog, Events, Ecommerce,...), you can also add them.

Default Options
===============

First try to construct the spirit of your theme by enabling a nice set of Odoo default options.
This also allows you to ensure two things:

#. You do not re-invent something which already exists. If Odoo provides an option to have a border
   on the footer, don’t recode it yourself. Enable it, then extend it if needed.
#. You ensure that the user can still use all of Odoo's features with your theme. Again, if Odoo
   provides an option to have a border on the footer and that you recode it yourself, you may break
   the default option or make it useless, giving the user a bad experience. Your option might also
   not work as well with all the other Odoo features relying on it.

Odoo Variables
--------------

Odoo declares many CSS rules, most being entirely customizable by overriding the related SCSS
variables. This can be done by creating a `primary_variables.scss` file and adding it to the
`_assets_primary_variables` bundle.

**Declaration**

.. code-block:: python
    :caption: ``/website_airproof/__manifest__.py``

    'assets': {
       'web._assets_primary_variables': [
          ('prepend', 'website_airproof/static/src/scss/primary_variables.scss'),
       ],
    },

By reading the source code, variables related to options are easily spottable.

.. code-block:: xml

   <we-button title="..."
   data-name="..."
   data-customize-website-views="..."
   data-customize-website-variable="'Sidebar'"
   data-img="..."/>

These variables can be overridden through the `$o-website-value-palettes` map.

Global
~~~~~~

Declaration
***********

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          // Templates
          // Colors
          // Fonts
          // Buttons
          // ...
       ),
    );

.. tip::
   That file must only contain definitions and overrides of SCSS variables and mixins

.. example::
   https://github.com/odoo/odoo/blob/15.0/addons/website/static/src/scss/primary_variables.scss#L1954

Fonts
~~~~~

You can embed any font on your website.
The Website Builder will automatically make it available in the font selector.

Declaration
***********

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-theme-font-configs: (
       <font-name>: (
          'family': <css font family list>,
          'url' (optional): <related part of Google fonts URL>,
          'properties' (optional): (
             <font-alias>: (
                <website-value-key>: <value>,
                ...,
             ),
          ...,
       )
    )

Use
***

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'font':                             '<font-name>',
          'headings-font':                    '<font-name>',
          'navbar-font':                      '<font-name>',
          'buttons-font':                     '<font-name>',
       ),
    );

Google Fonts
************

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-theme-font-configs: (
       'Poppins': (
          'family':                         ('Poppins', sans-serif),
          'url':                            'Poppins:400,500',
          'properties' : (
             'base': (
                'font-size-base':           1rem,
             ),
          ),
       ),
    );

Custom Fonts
************

First, create a specific SCSS file to declare your custom font(s).

.. code-block:: python
    :caption: ``/website_airproof/__manifest__.py``

    'assets': {
       'web.assets_frontend': [
          'website_airproof/static/src/scss/font.scss',
       ],
    },

Then, use the `@font-face` rule to allow you custom font(s) to be loaded on your website.

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/font.scss``

    @font-face {
       font-family: '<font-name>';
       ...
    }

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-theme-font-configs: (
       'Proxima Nova': (
          'family':                         ('Proxima Nova', sans-serif),
          'properties' : (
             'base': (
                'font-size-base':           1rem,
             ),
          ),
       ),
    );

Colors
~~~~~~

Odoo relies on five named colors to be used by the Website Builder. By defining those in your theme,
you ensure a consistent colored Odoo theme.

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Color
     - Description
   * - o-color-1
     - Primary
   * - o-color-2
     - Secondary
   * - o-color-3
     - Extra
   * - o-color-4
     - Whitish
   * - o-color-5
     - Blackish

.. image:: theming/theme-colors.png
   :align: left
   :alt: Theme colors
   :width: 300

Declaration
*************

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-color-palettes: map-merge($o-color-palettes,
       (
          'airproof': (
             'o-color-1':                    #bedb39,
             'o-color-2':                    #2c3e50,
             'o-color-3':                    #f2f2f2,
             'o-color-4':                    #ffffff,
             'o-color-5':                    #000000,
          ),
       )
    );

Add the palette you have just created to the list of palettes offered by the Website Builder.

.. code-block:: scss

   $o-selected-color-palettes-names: append($o-selected-color-palettes-names, 'airproof');

Use
***

Shapes
~~~~~~

Bootstrap Variables
-------------------

Views
-----

Assets
======

Styles
------

Interactivity
-------------
