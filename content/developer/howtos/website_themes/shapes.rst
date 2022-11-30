======
Shapes
======

Shapes are very useful if you want to add some personality to your website.

In this chapter, you will learn how to :

- Add standard background shape and image shape to your content
- Add a custom background shape
- Add a custom image shape

Background Shapes
=================

Background shapes are SVG images that you can add as a decorative background in your different
sections. Each shape has one or several customizable colours. Some of them are also animated.

Standard
--------

By default, there is a large selection of background shapes that can be used in your website.

Use

.. code-block:: xml

    <section data-oe-shape-data="{'shape':'web_editor/Zigs/06'}">
        <div class="o_we_shape o_web_editor_Zigs_06"/>
        <div class="container">
            <!-- Content -->
        </div>
    </section>

- `data-oe-shape-data` : the location of your shape.

You can flip the shape on the horizontally or vertically using the X or Y axis

.. code-block:: xml

    <section data-oe-shape-data="{'shape':'web_editor/Zigs/06','flip':[x,y]}">
        <div class="o_we_shape o_we_flip_x o_we_flip_y o_web_editor_Zigs_06"/>
        <div class="container">
            <!-- Content -->
        </div>
    </section>

You can also change the default colour mapping of your shape.

First, put the c* colour (here `4`).

Then, the replacement colour (here `3`). These replacement colours also range from 1 to 5:

- `1` = background colour of the colour preset 1 (o-cc1)
- `2` = background colour of the colour preset 2 (o-cc2)
- `3` = background colour of the colour preset 3 (o-cc3)
- `4` = background colour of the colour preset 4 (o-cc4)
- `5` = background colour of the colour preset 5 (o-cc5)

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/boostrap_overridden.scss``

    $o-bg-shapes: change-shape-colors-mapping('web_editor', 'Zigs/06', (4: 3, 5: 1));

Add extra colours mapping: this allows you to add a colour variant to the template of a shape,
while keeping the original as well.

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/boostrap_overridden.scss``

    $o-bg-shapes: add-extra-shape-colors-mapping('web_editor', 'Zigs/06', 'second', (4: 3, 5: 1));

.. code-block:: xml

    <section data-oe-shape-data="{'shape':'web_editor/Zigs/06'}">
        <div class="o_we_shape o_web_editor_Zigs_06 o_second_extra_shape_mapping"/>
        <div class="container">
            <!-- Content -->
        </div>
    </section>

Custom
------

Sometimes, the standard shapes might not be enough and your design might require one or several
custom shapes to be created. No problem here!

Firstly, you need to create a SVG file for your shape, like this one.

.. code-block:: xml
    :caption: ``/website_airproof/static/shapes/hexagons/01.svg``

    <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="86" height="100">
        <polygon points="0 25, 43 0, 86 25, 86 75, 43 100, 0 75" style="fill: #3AADAA;"/>
    </svg>

Make sure to use colors from the default Odoo palette for your shape.

.. code-block:: scss

    default_palette = {
        '1': '#3AADAA',
        '2': '#7C6576',
        '3': '#F6F6F6',
        '4': '#FFFFFF',
        '5': '#383E45',
    }

Declare your shape file.

.. code-block:: xml
    :caption: ``/website_airproof/data/shapes.xml``

    <record id="shape_hexagon_01" model="ir.attachment">
        <field name="name">01.svg</field>
        <field name="datas" type="base64" file="website_airproof/static/shapes/hexagons/01.svg"/>
        <field name="url">/web_editor/shape/illustration/hexagons/01.svg</field>
        <field name="public" eval="True"/>
    </record>

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Field
     - Description
   * - name
     - Name of the shape
   * - datas
     - The path to the shape
   * - url
     - ...
   * - public
     - Make the shape available for later edit

Define the styles of your shape.

.. code-block:: scss
    :caption: ``/website_airproof/static/src/scss/primary_variables.scss``

    $o-bg-shapes: map-merge($o-bg-shapes,
        (
            'illustration': map-merge(
                map-get($o-bg-shapes, 'illustration') or (),
                (
                    'hexagons/01': ('position': center center, 'size': auto 100%, 'colors': (1), 'repeat-y': false),
                ),
            ),
        )
    );

- `hexagons/01`: corresponds to the location of your file in the :file:`shapes` folder.
- `colors`: The color c* you want it to have (this will override the color you specified in your svg).

Last but not least, add your shape to the list of shapes available in the Website Builder.

.. code-block:: xml
    :caption: ``/website_airproof/views/snippets/options.xml``

    <template id="snippet_options_background_options" inherit_id="website.snippet_options_background_options" name="Shapes">
        <xpath expr="//*[hasclass('o_we_shape_menu')]/*[last()]" position="after">
            <we-select-page string="Theme">
                <we-button data-shape="illustration/hexagons/01" data-select-label="Hexagon 01"/>
            </we-select-page>
        </xpath>
    </template>

Your custom shape is now ready and you can use it in the same way as the other standard shapes.

Image Shapes
============

Image shapes are SVG that you can add as a clipping mask on your website images. Some shapes have
customizable colours and some are animated.

Standard
--------

By default, there is a large selection of image shapes that can be used in your website.

Use

.. code-block:: xml

    <img src="..."
        class="img img-fluid mx-auto"
        alt="..."
        data-shape="web_editor/solid/blob_2_solid_str"
        data-shape-colors="#35979C;;;;"
    >

.. list-table::
   :header-rows: 1
   :stub-columns: 1
   :widths: 20 80

   * - Attribute
     - Description
   * - data-shape
     - Location of the shape
   * - data-shape-colors
     - The colors applied to your shape

.. warning::
   It might be possible that your image shape is not applied after your changes. Just open the
   Website Builder and save the page to force the loading of the shape.

Custom
------

Just like background shapes, it is possible to create your own image shapes : just follow these
steps !

First, you have to create a SVG file for your shape with a ration 1:1 and  `id="filterPath"` on the
path. You can then convert your SVG file using this `script <https://github.com/odoo/odoo/tree/dbcb37ec80bb0fef0115c879c15bdc7073894290/addons/web_editor/static/image_shapes>`_. Once it’s done, you can add your shape
in your module :

`/website_airproof/static/image_shapes/blob/01.svg`

Next, you can record your shape.

.. code-block:: xml
    :caption: ``/website_airproof/data/shapes.xml``

    <record id="img_shape_blob_01" model="ir.attachment">
        <field name="name">01.svg</field>
        <field name="datas" type="base64" file="website_airproof/static/image_shapes/blob/01.svg"/>
        <field name="url">/web_editor/shape/illustration/blob/01.svg</field>
        <field name="public" eval="False"/>
    </record>

Finally, you need to add the option in the Website Builder.

.. code-block:: xml
    :caption: ``/website_airproof/views/snippets/options.xml``

    <template id="image_shapes_options" inherit_id="web_editor.snippet_options" name="Airproof - Image Shapes">
        <xpath expr="//*[@data-name='shape_img_opt']/*[last()]" position="after">
            <we-select-page string="Theme">
                <we-button data-set-img-shape="website_airproof/blob/01" data-select-label="Blob 1"/>
            </we-select-page>
        </xpath>
    </template>

You can now use your custom shape the same way you would use a standard shape.
