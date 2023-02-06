==========
Animations
==========

Eye-catching animations can bring your website to life.

On appearance
=============

In standard, you can add animations to columns when they appear, thanks to the Website Builder. Odoo
detects when your element is in the viewport and launches the animation. A large selection of
animations is available:

- Fade in
- Bounce in
- Rotate in
- Zoom in
- …

You can easily define an animation on a column in your custom theme. You need to add two classes:
`o_animate` and `o_anim_fade_in`. The second class changes depending on the type of animation you
want to use.

**Use**

.. code-block:: xml

   <div class="col-lg-6 o_animate o_anim_fade_in">
       <h2>A Section Subtitle</h2>
       <p>Write one or two paragraphs describing your product or services.</p>
   </div>
