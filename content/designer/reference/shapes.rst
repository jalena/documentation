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

- 1 = background colour of the colour preset 1 (o-cc1)
- 2 = background colour of the colour preset 2 (o-cc2)
- 3 = background colour of the colour preset 3 (o-cc3)
- 4 = background colour of the colour preset 4 (o-cc4)
- 5 = background colour of the colour preset 5 (o-cc5)

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

Image Shapes
============

Standard
--------

Custom
------
