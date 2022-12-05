==========
Navigation
==========

With the Website Builder, you can easily modify the navigation to fit your needs.

In this chapter, you will learn how to :

- Delete and create menu items.
- Create a dropdown menu.
- Create a mega menu.

Default
=======

Odoo automatically generates some basic menu items depending on the apps you installed. For example,
the website app adds two items in the main. These items are linked to pages, also automatically
created.

**Delete default menu items**

.. code-block:: xml
    :caption: ``/website_airproof/data/menu.xml``

    <!-- Contact us -->
    <delete model="website.menu" search="[('url','in', ['/', '/contactus']),
    ('website_id', '=', 1)]"/>

.. code-block:: xml

    <!-- Shop -->
    <delete model="website.menu" search="[('url','in', ['/', '/shop']),
    ('website_id', '=', 1)]"/>

Menu Item
=========

**Declaration**

.. code-block:: xml
    :caption: ``/website_airproof/data/menu.xml``

    <record id="menu_about_us" model="website.menu">
        <field name="name">About us</field>
        <field name="url">/about-us</field>
        <field name="parent_id" search="[
            ('url', '=', '/default-main-menu'),
            ('website_id', '=', 1)]"/>
        <field name="website_id">1</field>
        <field name="sequence" type="int">10</field>
    </record>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Field
     - Description
   * - name
     - Link text.
   * - url
     - Value of the href attribute.
   * - parent_id
     - The menu in which the item will be added.
   * - website_id
     - The website in which the item will be added.
   * - sequence
     - Defines the link's position in the top menu.

New window
----------

Open the link's URL in a new tab.

.. code-block:: xml

    <record id="..." model="website.menu">
        <field name="new_window" eval="True"/>
    </record>

External Links
--------------

Add a link to an external website.

.. code-block:: xml

    <record id="..." model="website.menu">
        <field name="url">https://www.odoo.com</field>
    </record>

Anchor
------

Link to a specific section of a page.

.. code-block:: xml

    <record id="..." model="website.menu">
        <field name="url">/about-us#our-team</field>
    </record>

Dropdown Menu
=============

**Declaration**

.. code-block:: xml
    :caption: ``/website_airproof/data/menu.xml``

    <record id="menu_services" model="website.menu">
        <field name="name">Services</field>
        <field name="website_id">1</field>
        <field name="parent_id" search="[
            ('url', '=', '/default-main-menu'),
            ('website_id', '=', 1)]"/>
        <field name="sequence" type="int">...</field>
    </record>

Add an item to a dropdown menu.

.. code-block:: xml

    <record id="menu_services_item_1" model="website.menu">
        <field name="name">Item 1</field>
        <field name="url">/dropdown/item-1</field>
        <field name="website_id">1</field>
        <field name="parent_id" ref="website_airproof.menu_services"/>
        <field name="sequence" type="int">...</field>
    </record>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Field
     - Description
   * - parent_id
     - The dropdown in which the item will be added.

Mega Menu
=========

A mega menu is a dropdown menu with extra possibilities. It's not just a list of links. In a mega
menu, you can use any kind of content (texts, images, icons,...).

**Declaration**

.. code-block:: xml
    :caption: ``/website_airproof/data/menu.xml``

    <record id="menu_mega_menu" model="website.menu">
        <field name="name">Mega Menu</field>
        <field name="url">/mega-menu</field>
        <field name="parent_id" search="[
            ('url', '=', '/default-main-menu'),
            ('website_id', '=', 1)]"/>
        <field name="website_id">1</field>
        <field name="sequence" type="int">..</field>
        <field name="is_mega_menu" eval="True"/>
        <field name="mega_menu_classes">...</field>
        <field name="mega_menu_content" type="html">
            <!-- Content -->
        </field>
    </record>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Field
     - Description
   * - is_mega_menu
     - Enable the mega menu feature.
   * - mega_menu_classes
     - Custom classes to be added to the main element.
   * - mega_menu_content
     - Default content of the mega menu.
