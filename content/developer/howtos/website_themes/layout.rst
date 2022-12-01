======
Layout
======

In this chapter, you will learn how to:

- Create a custom header
- Create a custom footer
- Modify a standard template
- Add a copyright section
- Improve your website's responsiveness

Default
=======

An Odoo page is the visual result of combining two kinds of elements, cross-pages and unique. By
default, Odoo provides you with a Header, a Footer (cross-pages), and a unique main element that
contains the content that makes your page unique.

.. code-block:: xml

    <div id="wrapwrap">
       <header/>
          <main>
             <div id="wrap" class="oe_structure">
                <!-- Page Content -->
             </div>
          </main>
       <footer/>
    </div>

.. note::
   Cross-pages elements will be the same on every page. Unique elements are related to a specific
   page only.

Any Odoo XML file starts with encoding specifications. After that, you must write your code inside
a `<odoo>` tag.

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8" ?>
    <odoo>
       ...
    </odoo>

Xpath
=====

XPath (XML Path Language) is an expression language that enables you to navigate through elements
and attributes in an XML document easily. We are going to use XPath to extend standard Odoo
templates.

A view will be coded like this:

.. code-block:: xml

    <template id="..." inherit_id="..." name="...">
       <!-- Content -->
    </template>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Attribute
     - Description
   * - id
     - Name of the modified view
   * - inherited_id
     - Name of the standard view

For each Xpath, we will play with two attributes: **expression** and **position**.

**Example**

.. code-block:: xml
    :caption: ``/website_airproof/views/website_templates.xml``

    <template id="layout" inherit_id="website.layout" name="Welcome Message">
       <xpath expr="//header" position="before">
          <!-- Content -->
       </xpath>
    </template>

This XPath will add a welcome message just before the page content. You can do a lot more things
with Xpath.

.. warning::
   Be careful replacing default elements attributes. As your theme will extend the default one,
   your changes will take priority in any future Odoo update.

.. note::
   Every time you create a new template or a new record, you will have to update your module.

Expression
----------

XPath uses path expressions to select nodes in an XML document. The node is selected by following
a path or steps. Selectors will be used inside the expression to target the right element. The most
useful selectors are listed below:

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Descendent selectors
     - Description
   * - /
     - Selects from the root node
   * - //
     - Selects nodes in the document from the current node that match the selection no matter where
       they are

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Attribute selectors
     - Description
   * - \*[@id="id"]
     - Selects a specific id
   * - \*[hasclass("class")]
     - Selects a specific class
   * - \*[@name="name"]
     - Selects a tag with a specific name
   * - \*[@t-call="t-call"]
     - Selects a specific t-call

Position
--------

The position will define where the code will be placed inside the template. The possible values are
listed below:

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Position
     - Description
   * - replace
     - Replaces the targeted node with the Xpath content
   * - inside
     - Adds the Xpath content inside the targeted node
   * - before
     - Adds the Xpath content before the targeted node
   * - after
     - Adds the Xpath content after the targeted node
   * - attributes
     - Adds the Xpath content inside an attribute

Examples
--------

This Xpath will add a `<div>` before the `<nav>` that is a direct child of the `<header>`.

.. code-block:: xml

    <xpath expr="//header/nav" position="before">
       <div>Some content before the header</div>
    </xpath>

This Xpath will add `x_airproof_header` in the class attribute of the header.

.. code-block:: xml

    <xpath expr="//header" position="attributes">
       <attribute name="class" add="x_airproof_header" remove="" separator=" "/>
    </xpath>

This Xpath will remove the first element with a `.breadcrumb` class.

.. code-block:: xml

    <xpath expr="//*[hasclass('breadcrumb')]" position="replace"/>

This Xpath will add a extra `<li>` element after the last child of the `<ul>` element.

.. code-block:: xml

    <xpath expr="//ul" position="inside">
       <li>Last element of the list</li>
    </xpath>

You will find more details about Xpath in this `Cheat Sheet <https://devhints.io/xpath>`_.

Qweb
====

QWeb is the primary templating engine used by Odoo. It is an XML templating engine used mostly to
generate HTML fragments and pages.

.. seealso::
   Check our :doc:`documentation about Qweb <../../reference/frontend/qweb>`.

Background
==========

You can define a color or an image as the background of your website.

**Colors**

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-color-palettes: map-merge($o-color-palettes,
       (
          'airproof': (
             'o-cc1-bg':                     'o-color-5',
             'o-cc5-bg':                     'o-color-1',
          ),
        )
    );

**Image / Pattern**

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'body-image': '/website_airproof/static/src/img/background-lines.svg',
          'body-image-type': 'image' or 'pattern'
       )
    );

Header
======

By default, Odoo header contains a responsive navigation menu and the company's logo. You can
easily add new elements or create your own template.

Standard
--------

Enable one of the header default templates.

.. important::
   Don't forget that you may need to disable the active header template first.

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'header-template': 'Contact',
       ),
    );

.. code-block:: xml
    :caption: ``/website_airproof/data/presets.xml``

    <record id="website.template_header_contact" model="ir.ui.view">
       <field name="active" eval="True"/>
    </record>

Custom
------

Create your own template and add it to the list.

.. important::
   Don't forget that you may need to disable the active header template first.

**Option**

.. code-block:: xml
    :caption: ``/website_airproof/data/presets.xml``

    <template id="template_header_opt" inherit_id="website.snippet_options" name="Header Template - Option">
       <xpath expr="//we-select[@data-variable='header-template']" position="inside">
          <we-button title="airproof"
             data-customize-website-views="website_airproof.header"
             data-customize-website-variable="'airproof'"  data-img="/website_airproof/static/src/img/wbuilder/template_header_opt.svg"/>
       </xpath>
    </template>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Attribute
     - Description
   * - data-customize-website-views
     - The template to enable
   * - data-customize-website-variable
     - The name given to the variable
   * - data-img
     - The thumbnail it will display in the choices of header offered in the Website Builder UI

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'header-template': 'airproof',
       ),
    );

**Layout**

.. code-block:: xml
    :caption: ``/website_airproof/views/website_templates.xml``

    <record id="header" model="ir.ui.view">
       <field name="name">Airproof Header</field>
       <field name="type">qweb</field>
       <field name="key">website_airproof.header</field>
       <field name="inherit_id" ref="website.layout"/>
       <field name="mode">extension</field>
       <field name="arch" type="xml">
          <xpath expr="//header//nav" position="replace">
             <!-- Static Content -->
             <!-- Components -->
             <!-- Editable areas -->
          </xpath>
       </field>
    </record>

Components
----------

In your custom header, you can call several sub-templates using the `t-call` directive from QWeb:

Logo (see :ref:`next section <website_themes/layout/logo>` to learn how to record it)

.. code-block:: xml

    <t t-call="website.placeholder_header_brand">
       <t t-set="_link_class" t-valuef="..."/>
    </t>

Menu

.. code-block:: xml

    <t t-foreach="website.menu_id.child_id" t-as="submenu">
       <t t-call="website.submenu">
          <t t-set="item_class" t-valuef="nav-item"/>
          <t t-set="link_class" t-valuef="nav-link"/>
       </t>
    </t>

Sign In

.. code-block:: xml

    <t t-call="portal.placeholder_user_sign_in">
       <t t-set="_item_class" t-valuef="nav-item"/>
       <t t-set="_link_class" t-valuef="nav-link"/>
    </t>

User dropdown

.. code-block:: xml

    <t t-call="portal.user_dropdown">
       <t t-set="_user_name" t-value="true"/>
       <t t-set="_icon" t-value="false"/>
       <t t-set="_avatar" t-value="false"/>
       <t t-set="_item_class" t-valuef="nav-item dropdown"/>
       <t t-set="_link_class" t-valuef="nav-link"/>
       <t t-set="_dropdown_menu_class" t-valuef="..."/>
    </t>

Language selector

.. code-block:: xml

    <t t-call="website.placeholder_header_language_selector">
       <t t-set="_div_classes" t-valuef="..."/>
    </t>

Call to action

.. code-block:: xml

    <t t-call="website.placeholder_header_call_to_action">
       <t t-set="_div_classes" t-valuef="..."/>
    </t>

Navbar toggler

.. code-block:: xml

    <t t-call="website.navbar_toggler">
       <t t-set="_toggler_class" t-valuef="..."/>
    </t>

.. _website_themes/layout/logo:

Logo
----

Record the logo of your website in the database.

.. code-block:: xml
    :caption: ``/website_airproof/data/images.xml``

    <record id="website.default_website" model="website">
       <field name="logo" type="base64" file="website_airproof/static/src/img/content/logo.png"/>
    </record>

Footer
======

By default, Odoo footer contains a section with some static content. You can easily add new elements
or create your own template.

Standard
--------

Enable one of the default footer templates. Don't forget that you may need to disable the active
footer template first.

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'header-template': 'Contact',
       ),
    );

.. code-block:: xml
    :caption: ``/website_airproof/data/presets.xml``

    <record id="website.template_header_contact" model="ir.ui.view">
       <field name="active" eval="True"/>
    </record>

Custom
------

Create your own template and add it to the list. Don't forget that you may need to disable the
active footer template first.

**Option**

.. code-block:: xml
    :caption: ``/website_airproof/data/presets.xml``

    <template id="template_header_opt" inherit_id="website.snippet_options" name="Footer Template - Option">
       <xpath expr="//we-select[@data-variable='footer-template']" position="inside">
          <we-button title="airproof"
             data-customize-website-views="website_airproof.footer"
             data-customize-website-variable="'airproof'"
             data-img="/website_airproof/static/src/img/wbuilder/template_header_opt.svg"/>
       </xpath>
    </template>

**Declaration**

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-website-values-palettes: (
       (
          'footer-template': 'airproof',
       ),
    );

**Layout**

.. code-block:: xml
    :caption: ``/website_airproof/views/website_templates.xml``

    <record id="footer" model="ir.ui.view">
       <field name="name">Airproof Footer</field>
       <field name="type">qweb</field>
       <field name="key">website_airproof.footer</field>
       <field name="inherit_id" ref="website.layout"/>
       <field name="mode">extension</field>
       <field name="arch" type="xml">
          <xpath expr="//div[@id='footer']" position="replace">
             <div id="footer" class="oe_structure oe_structure_solo" t-ignore="true" t-if="not no_footer">
                <!-- Content -->
             </div>
          </xpath>
       </field>
    </record>

Copyright
=========

There is only one template available at the moment for the copyright bar. To replace the content or
modify the structure.

.. code-block:: xml
    :caption: ``/website_airproof/views/website_templates.xml``

    <template id="copyright" inherit_id="website.layout">
       <xpath expr="//div[hasclass('o_footer_copyright')]" position="replace">
          <div class="o_footer_copyright" data-name="Copyright">
             <!-- Content -->
          </div>
       </xpath>
    </template>

Drop Zone
=========

Instead of defining the complete layout for a page, you can create building blocks (snippets) and
let the user choose where to "drag and drop" them, creating the page layout on their own. We call
this modular design.

Define an empty area that the user can fill with snippets.

.. code-block:: xml

    <div id="oe_structure_layout_01" class="oe_structure"/>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Class
     - Description
   * - oe_structure
     - Define a drag and drop area for the user
   * - oe_structure_solo
     - Only one snippet can be dropped in this area
   * - oe_empty
     - ...
   * - oe_structure_not_nearest
     - ...

You can also populate an existing drop zone with your content:

.. code-block:: xml

    <template id="oe_structure_layout_01" inherit_id="..." name="...">
       <xpath expr="//*[@id='oe_structure_layout_01']" position="replace">
          <div id="oe_structure_layout_01" class="oe_structure oe_structure_solo">
             <!-- Content -->
          </div>
       </xpath>
    </template>

Responsive
==========

Some hints to help you make your website responsive.

Bootstrap
---------

**Breakpoints**

https://getbootstrap.com/docs/4.6/layout/overview/#responsive-breakpoints

**Display**

https://getbootstrap.com/docs/4.6/utilities/display/

**Font-size**

As of v4.3.0, Bootstrap ships with the option to enable responsive font sizes, allowing text to
scale more naturally across device and viewport sizes. RFS can be enabled by changing the
`$enable-responsive-font-sizes` Sass variable to true.

.. seealso::

   - Check the `Responsive Font Size <https://github.com/twbs/rfs/tree/v8.1.0>`_


Website Builder
---------------

Hide a specific `<section>` on mobile:

.. code-block:: xml

    <section class="d-none d-md-block">
       <!-- Content -->
    </section>

Make `.pt*` and `.pb*` classes responsive:

.. code-block:: scss

    @include media-breakpoint-down(lg) {
       @for $i from 0 through (256 / 8) {
          @include o-vspacing($i * 8, .5);
       }
    }
