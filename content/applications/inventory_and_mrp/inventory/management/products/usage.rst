==========================================
Units of measure, packages, and packagings
==========================================

In Odoo, there are a variety of ways to specify the amount of products being bought, stocked, and
sold. Units of measure, packages, and packagings are all available to streamline inventory flows,
allowing for a variety of configurations for products as they enter and leave the warehouse.

Units of measure
================
*Units of measure* refers to different measurable quantities used to handle products, such as units,
weight, time, size, etc. For example, different units of measure for weight can be kilos, pounds,
ounces, grams, etc. In Odoo, stock is easily managed and products are more efficiently purchased
from suppliers by specifying different units of measure for both purchasing and selling.

.. image:: usage/rope-14.png
   :align: center
   :alt: Specify unit of measure for selling a product vs purchasing.

Once a product has a :guilabel:`Unit of Measure` and a :guilabel:`Purchase UoM` set on the product
form, Odoo will automatically convert the different units in the product's purchase/sales orders and
the orders' respective delivery orders/receipts. The only condition is that all of the units have to
be in the *same category* (Unit, Weight, Volume, Length, etc.). For example, a product can have its
:guilabel:`Unit of Measure` field set to `feet (ft)` and its :guilabel:`Purchase UoM` set to
`centimeters (cm)`. When a :abbr:`PO (purchase order)` is created for that product, it will list the
quantity in centimeters. Then, when the abbr:`PO (purchase order)` is confirmed, Odoo automatically
generates a receipt and converts the centimeters to feet. The receipt will list the quantity in
feet.

Packages
========
A *package* refers to the physical container that holds one or several products from a picking.
Packages can be a reusable or disposable (shipping) box, and are **not** specific to a product.
Reusable boxes temporarily hold products as they are picked, before being brought to either a
packing or shipping area. Disposable boxes are the actual shipping containers (cardboard boxes,
envelopes, shipping bags, etc.) that will be used to ship the products out to customers.
When a sales order that contains multiple items is ready for delivery, the quantities can be
separated into different packages to accommodate the products. For example, a sales order that has
20 boxes of pencils and 4 boxes of erasers can be separated into two separate packages, each
containing 10 boxes of pencils and 2 boxes of erasers.

.. note::
   The products do not have to be divided equally. Products can be divided into as many packages
   that are needed to accommodate the sales order.

In Odoo, the quantity of products in each package needs to be recorded so there is a full history
for each product, including which package each item is shipped out in. To use packages, ensure the
:guilabel:`Packages` option is enabled in :menuselection:`Inventory --> Configuration --> Settings
--> Operations`.

On a delivery order, separate the products into different packages by clicking on the
:guilabel:`Detailed Operations` icon next to each product. A pop up will appear, allowing the
amounts to be specified for each product, and what pack(s) the product(s) will go into. Add a line
for each additional package to be used, and specify the amount of the product to go in the specific
package. Once all the products for each line has been entered, click :guilabel:`Confirm`. Repeat
this for all products on the delivery order.

.. image:: usage/packages-detailed-14-15.png
   :align: center
   :alt: Detailed operations where the amount of product going in a pack can be specified.

The done quantity on the delivery order will be updated to show the products selected in the various
packages. When the done amount matches the demand amount, click :guilabel:`Put In Pack`, then
:guilabel:`Validate` the delivery order. Clicking on the :guilabel:`Packages` smart button will show
the packages used in the delivery order.

.. image:: usage/packages-14-15-out.png
   :align: center
   :alt: Put in pack once the done amount matches the demand.

Packagings
==========

*Packaging* is product specific, and refers to a disposable container that holds several units of a
specific product. Unlike packages, packagings cannot be reusable, and each specific packaging must
be defined on the individual product form. For example, different packages for cans of soda can be
configured as a 6-pack, a 12-pack, or a case of 36. Each flavor of soda would need a 6, 12, and 36
can packaging configured on the individual product since packagings are product specific, not
generic.

To use packagings, ensure :guilabel:`Product Packagings` is enabled under :menuselection:`Inventory
--> Configuration --> Settings --> Products`. In Odoo, product packagings are used on sales/purchase
orders and inventory transfers.

To create packagings, click :guilabel:`Edit` on the product page. In the inventory tab, click
:guilabel:`Add a line`. Enter the information for each packaging, specifying the name, type of
packaging, and the amount in each pack, then click :guilabel:`Save`.

.. image:: usage/grape-soda-14.png
   :align: center
   :alt: Packaging specified on the product page form inventory tab.

To view all packagings that have been created, go to :menuselection:`Inventory --> Configuration -->
Product Packagings`, and a list will appear with all packagings created for all products. In this
example, there are two different kinds of sodas with three types of packagings configured for each.
New packagings can be created from this report by clicking :guilabel:`Create`.

.. image:: usage/packagings-14.png
   :align: center
   :alt: List of different packagings for products.

When creating a sales order, specify the packagings that should be used for the product. In this
example, 18 cans of soda will be picked and/or packed using three 6-pack packagings.

 .. image:: usage/packagings-sales-order-14.png
   :align: center
   :alt: Sales order showing the packages being used.

.. tip::
   Packaging is also useful during product procurement at the reception level when used in
   conjunction with Odoo Barcode. When scanning the barcode of the packaging, Odoo automatically
   adds the number of units contained in the packing on the picking.

.. seealso::
   - :doc:`uom`
