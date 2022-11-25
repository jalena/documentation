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

**Declaration**

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

**Declaration**

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

**Declaration**

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

**Use**

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

..
   .. image:: theming/theme-colors.png
      :align: left
      :alt: Theme colors
      :width: 300

**Declaration**

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

**Use**

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'color-palettes-name':              'airproof',
       ),
    );

Color Combinations
^^^^^^^^^^^^^^^^^^

Based on the five colors palette previously defined, the website builder will automatically
generates five color combinations. They come with a background color, a text color, headings colors,
a link color, primary and secondary button colors which will later be possible to customize by the
user.

..
   .. image:: theming/theme-colors-big.png
      :align: left
      :alt: Theme colors
      :width: 300

The colors used in a color combination are accessible and possible to override through the BS
`$colors map`, with a specific prefix (`o-cc` for `color combination`).

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-color-palettes: map-merge($o-color-palettes,
       (
          'airproof': (

             'o-cc*-bg':                     'o-color-*',
             'o-cc*-text':                   'o-color-*',
             'o-cc*-headings':               'o-color-*',
             'o-cc*-h2':                     'o-color-*',
             'o-cc*-h3':                     'o-color-*',
             'o-cc*-h4':                     'o-color-*',
             'o-cc*-h5':                     'o-color-*',
             'o-cc*-h6':                     'o-color-*',
             'o-cc*-link':                   'o-color-*',
             'o-cc*-btn-primary':            'o-color-*',
             'o-cc*-btn-primary-border':     'o-color-*',
             'o-cc*-btn-secondary':          'o-color-*',
             'o-cc*-btn-secondary-border':   'o-color-*',

          ),
       )
    );

.. admonition:: Reference

   https://github.com/odoo/odoo/blob/15.0/addons/web_editor/static/src/scss/web_editor.common.scss#L708

.. admonition:: Demo page

   The Website Builder automatically generates a page for you to see the color combinations of the
   theme color palette:

   http://localhost:8069/website/demo/color-combinations

Bootstrap Variables
-------------------

Odoo includes Bootstrap by default.
This means that you can take advantage of all variables and mixins of the framework.

If Odoo does not provide the variable you are looking for, then try to find a Bootstrap variable
that allows it. Indeed all Odoo layouts respect Bootstrap structures and use Bootstrap components or
extensions of them. So if you customize a bootstrap variable, you add a generic style for the whole
user website.

Bootstrap values must not be overridden in the `primary_variables.scss` file but in another
dedicated file, added to the `_assets_frontend_helpers` bundle.

**Declaration**

.. code-block:: python
    :caption: ``/website_airproof/__manifest__.py``

    'assets': {
       'web._assets_frontend_helpers': [
          ('prepend', 'website_airproof/static/src/scss/bootstrap_overridden.scss'),
       ],
    },

**Use**

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/bootstrap_overridden.scss``

    // Typography
    $h1-font-size:                 4rem !default;

    // Navbar
    $navbar-nav-link-padding-x:    1rem!default;

    // Buttons + Forms
    $input-placeholder-color:      o-color('o-color-1') !default;

    // Cards
    $card-border-width:            0 !default;

.. tip::
   That file must only contain definitions and overrides of SCSS variables and mixins.

.. warning::
   Make sure not to override Bootstrap variables that depend on Odoo variables.
   Otherwise, you might break the possibility for the user to customize them using the Odoo Website
   Builder.

.. admonition:: Reference

   https://github.com/odoo/odoo/blob/15.0/addons/website/static/src/scss/bootstrap_overridden.scss

.. admonition:: Demo page

   http://localhost:8069/website/demo/bootstrap

Views
-----

For some options, in addition to the Website Builder variable, you also have to activate a specific
view.

By reading the source code, templates related to options are easily spottable.

.. code-block:: xml
    <we-button title="..."
       data-name="..."
       data-customize-website-views="website.template_header_default"
       data-customize-website-variable="'...'"
       data-img="..."/>

.. code-block:: xml
    <template id="..." inherit_id="..." name="..." active="True"/>
    <template id="..." inherit_id="..." name="..." active="False"/>

**Example**: Change menu items horizontal alignment

.. code-block:: xml
    :caption: ``/website_airproof/data/presets.xml``

    <record id="website.template_header_default_align_center" model="ir.ui.view">
       <field name="active" eval="True"/>
    </record>

The same logic can be used for others Odoo Apps as well.

**Example**: E-commerce - Display Products Categories

.. code-block:: xml

    <record id="website_sale.products_categories" model="ir.ui.view">
       <field name="active" eval="False"/>
    </record>

**Example**: Portal - Disable Language Selector

.. code-block:: xml

    <record id="portal.footer_language_selector" model="ir.ui.view">
       <field name="active" eval="False"/>
    </record>

Assets
======

For this part, we will refer to the  assets_frontend bundle, located in the web module. This bundle
specifies the list of assets loaded by the Website Builder, and our goal is to add our SCSS and JS
files to it.

Styles
------
The Odoo Website Builder and Bootstrap are great for defining the basic styles of your website.
But to provide a unique design, you need to go a step further.
For this, you can easily add any SCSS file to your theme.

**Declaration**

.. code-block:: python
    :caption: ``/website_airproof/__manifest__.py``

    'assets': {
       'web.assets_frontend': [
          'website_airproof/static/src/scss/theme.scss',
       ],
    },

Feel free to reuse the variables in your `theme.scss` file (both the ones you put in your bootstrap
file, and the ones used by odoo).

**Example**

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/theme.scss``

     blockquote {
       border-radius: $rounded-pill;
       color: o-color('o-color-3');
       font-family: o-website-value('headings-font');
    }

Interactivity
-------------

Odoo supports three different kinds of javascript files:

- `plain javascript files <https://www.odoo.com/documentation/15.0/developer/reference/frontend/javascript_modules.html#frontend-modules-plain-js>`_ (no module system),
- `native javascript module <https://www.odoo.com/documentation/15.0/developer/reference/frontend/javascript_modules.html#frontend-modules-native-js>`_.
- `Odoo modules <https://www.odoo.com/documentation/15.0/developer/reference/frontend/javascript_modules.html#frontend-modules-odoo-module>`_ (using a custom module system),

Most new Odoo javascript code should use the native javascript module system. This is simpler and
brings the benefits of a better developer experience with better integration with the IDE.

There is an essential point to know: Odoo needs to know which files should be translated into
`Odoo modules <https://www.odoo.com/documentation/15.0/developer/reference/frontend/javascript_modules.html#frontend-modules-odoo-module>`_ and which files should not be translated. This is an opt-in system: Odoo will look at
the first line of a JS file and check if it contains the string `@odoo-module`. If so, it will
automatically be converted to an Odoo module.

.. code-block:: javascript

    /** @odoo-module **/

**Declaration**

.. code-block:: python
    :caption: ``/website_airproof/__manifest__.py``

    'assets': {
       'web.assets_frontend': [
          'website_airproof/static/src/scss/theme.js',
       ],
    },

.. note::
   If you want to include files from an external library, you can add them into the /lib folder of
   your module.

.. admonition:: Reference

   https://www.odoo.com/documentation/developer/reference/frontend/javascript_reference.html