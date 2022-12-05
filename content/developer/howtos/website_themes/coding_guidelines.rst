=================
Coding Guidelines
=================

Proper code improves readability, eases maintenance, helps to debug, lowers complexity, and promotes
reliability. These guidelines should be applied to every new module and to all new development.

.. seealso::
   :doc:`Coding Guidelines <../../../contributing/development/coding_guidelines>`

Comment your code so that someone else can understand what you have done and why. Always write in
English, whether for comments, IDs, Classes, etc.

Indentation
===========

- Use 4 spaces per indentation level.
- Do not use tabs.
- Never mix spaces and tabs.

Files
=====

File naming is important to quickly find information through all modules. File names should only
contain lowercase, alphanumerics, and underscores: a-z, 0-9, _

Always add an empty newline at the end of your file. This can be easily configured in your IDE to be
done automatically.

XML
===

*XML IDs* of inheriting views should use the same *ID* as the original record. It helps to find all
inheritance at a glance. As final *XML IDs* are prefixed by the module that creates them, there is
no overlap.

.. code-block:: xml

    <template id="navbar_toggler" inherit_id="website.navbar_toggler"> ... </template>

SCSS
====

- Use Bootstrap native classes as much as possible.
- Prefix all your custom classes.
- Use underscore lowercase notation to name class, e.g.: `.x_nav`, `.x_nav_item`.
- Avoid using ID tag.

JS
==

- Use a linter (JSHint,...).
- Never add minified JavaScript libraries.
- Add `'use strict';` on top of every odoo JS module.
- Variables and functions should be *camelcased* (`myVariable`) instead of *snakecased* (`my_variable`).
- Do not name a variable event, use `ev.` This is to avoid bugs on non-Chrome browsers as Chrome is
  magically assigning a global event variable (so if you use the event variable without declaring
  it, it will be fine on chrome but crash on every other browser).
- Use strict comparisons (`===` instead of `==`).
- Double quotes for all textual strings (such as `"Hello"`), and single quotes for all other strings,
  such as a CSS selector `.x_nav_item`.
- Always use `this._super.apply(this, arguments)`;

.. seealso::
   - Detailed JS guidelines: `Odoo Wiki <https://github.com/odoo/odoo/wiki/Javascript-coding-guidelines>`_
   - Detailed Odoo Javascript framework: :doc:`Javascript Reference <../../reference/frontend/javascript_reference>`
