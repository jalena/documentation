==========
Animations
==========

Eye-catching animations can really bring your website to life and impress visitors.

On Appearance
=============

In standard, you can add animations on appearance to columns thanks to the Website Builder. Odoo
will detect when your element is in the viewport and launch the animation. There is a large
selection of animations available :

- Fade in
- Bounce in
- Rotate in
- Zoom in
- …

In your custom theme, you can easily define an animation on a column. You simply need to add two
classes : `o_animate` and `o_anim_fade_in`. The second class will change depending on the type of
animation you want to use;

**Use**

.. code-block:: xml

    <div class="col-lg-6 o_animate o_anim_fade_in">
        <h2>A Section Subtitle</h2>
        <p>Write one or two paragraphs describing your product or services.</p>
    </div>
