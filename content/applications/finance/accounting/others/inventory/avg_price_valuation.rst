=========================================
Calculate average price on returned goods
=========================================

.. _inventory/avg_cost/definition:

Average cost valuation (AVCO) is a method of accounting for a company's on-hand stock to reflect the
value of a company's assets. This method evaluates a product based on its average cost, divided by
the amount of stock on-hand. It is also *automatically updated* each time products are received.

Thus, how does returning a shipment to its supplier affect the average cost valuation and accounting
entries?

.. note::
   This document addresses a specific use case for theoretical purposes. Navigate :ref:`here
   <inventory/inventory_valuation_config>` for instructions on how to set up and use AVCO in Odoo.

.. seealso::
   - :ref:`Using inventory valuation<inventory/reporting/using_inventory_val>`
   - :ref:`Other inventory valuation methods<inventory/inventory_valuation_config/costing_methods>`

Define average cost
===================

The average cost method updates the inventory valuation of a company each time products are shipped
*into* the warehouse.

Odoo calculates the average cost of a product by:

#. Adding the old average cost and new price of the product, and
#. Dividing the sum by the total count of the product in the warehouse

.. _inventory/avg_cost/formula:

Formula
-------

When new products arrive, the new average cost for each product is recomputed using the formula:

.. math::
   Avg~Cost = \frac{Old~Qty \times Old~Avg~Cost + Incoming~Qty \times Purchase~Price}{Final~Qty}

- **Old Qty**: count of the product in stock the moment before receiving the new shipment.
- **Old Avg Cost**: average cost for a single product calculated in the previous inventory
  valuation.
- **Incoming Qty**: count of products in arriving in the new shipment.
- **Purchase Price**: estimated price of products at the reception of products (since vendor bills
  may arrive later). The amount includes not only the price for the products, but also added costs,
  such as shipping, taxes, and :ref:`landed costs<inventory/reporting/landed_costs>`. At
  reception of the vendor bill, this price is adjusted.

.. _inventory/avg_cost/definite_rule:

.. important::
   When products leave the warehouse, the average cost **does not** change. Read why this fails
   :ref:`here<inventory/avg_price/leaving_inventory>`.

.. _inventory/avg_cost/math_table:

Compute average cost
--------------------

To conceptualize how the average cost of a product changes each time a shipment arrives, the
following table contains different warehouse operations and stock moves. Each is a different example
of how the average cost valuation is affected.

+--------------------------------+---------------+-------------------+---------------+------------+
| Operation                      | Incoming Value| Inventory Value   | Qty On Hand   | Avg Cost   |
+================================+===============+===================+===============+============+
|                                |               | $0                | 0             | $0         |
+--------------------------------+---------------+-------------------+---------------+------------+
| 1. Receive 8 tables at $10/unit| +8*$10        | $80               | 8             | $10        |
+--------------------------------+---------------+-------------------+---------------+------------+
| 2. Receive 4 tables at $16/unit| +4*$16        | $144              | 12            | $12        |
+--------------------------------+---------------+-------------------+---------------+------------+
| 3. Deliver 10 tables           | -10*$12       | $24               | 2             | $12        |
+--------------------------------+---------------+-------------------+---------------+------------+

To start, there are 0 tables in stock, so all values are $0.

Stock products for the first time
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. At the first warehouse operation, `8` tables for `$10` each are shipped into the warehouse. The
   average cost is evaluated with the :ref:`formula<inventory/avg_cost/formula>`:

   .. math::
      Avg~Cost = \frac{0 + 8 \times $10}{8} = \frac{$80}{8} = $10

   #. Since *incoming quantity* of tables is `8` and the *purchase price* for each is `$10`,
   #. Multiply `8 * $10` to get `$80` in the numerator. (The inventory value of the previous
      operation is `$0`.)
   #. `$80` is divided by the total amount of tables to be stored, `8`.
   #. `$10` is the average cost of a single table from the first shipment.

Receive additional products
~~~~~~~~~~~~~~~~~~~~~~~~~~~

2. In the second reception, `4` tables are purchased for a price of `$16` each. Refer to the
   :ref:`formula<inventory/avg_cost/formula>` again to update the average cost:

   .. math::
      Avg~Cost = \frac{8 \times $10 + 4 \times $16}{8+4} = \frac{$144}{12} = $12

   #. *Incoming quantity* of tables is `4` and new *purchase price* `$16`.
   #. The *Old Qty* and *Old price* are `8` and `$10`, respectively.
   #. Thus, the numerator is found by adding the product's old inventory value(`$80`) to
      the incoming value (`4 * $16 = $64`). `$80 + $64 = $144`
   #. The numerator total (`$144`) is divided by the total on-hand stock, `8 + 4 = 12`.
   #. Thus, `$144 / 12 = $12` is the updated average cost per table at this time.

Deliver products to customers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

3. For outgoing shipments such as the third operation, the former average price is used as the
   product's **purchase price**. To confirm that sending a delivery has no effect on average cost,
   see:

   .. math::
      Avg~Cost = \frac{12 \times $12 + -10 \times $12}{12-10} = \frac{24}{2} = $12

   #. Because 10 *tables* are being sent out to customers, the *incoming quantity* is `-10`. The
      previous average cost (`$12`) is used in lieu of a vendor's *purchase price*.
   #. *Incoming inventory value* is `-10 * $12 = -$120`.
   #. The old *inventory value* (`$144`) is added to the *incoming inventory value* (`-$120`), so
      `$144 + -$120 = $24`.
   #. Only `2` tables remain after shipping out `10` tables from `12`. So the current *inventory
      value* (`$24`) is divided by the on-hand quantity(`2`).
   #. `$24 / 2 = $12`, which is the same average cost as the previous operation.

   .. note::
      This reifies the :ref:`assertion made<inventory/avg_cost/definite_rule>` that outbound
      products have no affect on the average cost.

Return items to supplier (use case)
===================================

Because the price paid to suppliers can differ from the price the product is valued at, Odoo handles
returned items in a particular way.

#. Products are returned to suppliers at the original purchase price, but
#. The internal cost valuation remains unchanged

The above :ref:`table<inventory/avg_cost/math_table>` is updated as follows:

+--------------------------------+---------------+-------------------+---------------+------------+
| Operation                      | Qty*Avg Cost  | Inventory Value   | Qty On Hand   | Avg Cost   |
+================================+===============+===================+===============+============+
|                                |               | $24               | 2             | $12        |
+--------------------------------+---------------+-------------------+---------------+------------+
| Return 1 table bought at $10   | -1\*$12       | $12               | 1             | $12        |
+--------------------------------+---------------+-------------------+---------------+------------+

In other words, returns are perceived by Odoo as another form of a product exiting the warehouse. To
Odoo, because the table is valued at $12 per unit, the inventory value is reduced by `$12` when the
product is returned; the initial purchase price of `$10` is unrelated to the table's average cost.

.. _inventory/avg_price/leaving_inventory:

Update cost when products leave: the consequences
-------------------------------------------------

As mentioned :ref:`previously<inventory/avg_cost/definite_rule>`, inconsistencies occur in a
company's inventory when the average cost valuation is recalculated on outgoing shipments.

To demonstrate this error, here is a scenario where 1 table is shipped to a customer and another is
returned to a supplier at the purchased price.

+------------------------------------------+---------------+-------------------+---------------+------------+
| Operation                                | Qty*Price     | Inventory Value   | Qty On Hand   | Avg Cost   |
+==========================================+===============+===================+===============+============+
|                                          |               | $24               | 2             | $12        |
+------------------------------------------+---------------+-------------------+---------------+------------+
| Ship 1 product to customer               | -1 \* $12     | $12               | 1             | $12        |
+------------------------------------------+---------------+-------------------+---------------+------------+
| Return 1 Product initially bought at $10 | -1 \* $10     | **$2**            | **0**         | $12        |
+------------------------------------------+---------------+-------------------+---------------+------------+

In the final operation above, the final inventory valuation for table is `$2` even though there are
`0` tables left in stock.

**Correct method**: Use the average cost to value the return. This does not mean the company gets
$12 back for a $10 purchase; the item returned for $10 is valued internally at $12. The inventory
value change represents a product worth $12 no longer being accounted for in company assets.

Considerations for Anglo-Saxon accounting
-----------------------------------------

Companies that use **Anglo-Saxon accounting** additionally keep a holding account that tracks the
amount to be paid to vendors. Once a vendor delivers an order, **Inventory value** increases
because products valued at that price has entered the stock. The holding account (called **stock
input**) is credited and only reconciled once the vendor bill is received.

Returning to the previous transactions involving tables, the chart below reflects journal entries
and accounts. The **stock input** account is meant to store the money intended to pay vendors when
the vendor bill has yet been received. There is an additional **price difference** account that
records the difference between price the product is valuated at and the price the vendor sold the
product for.

+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Operation                               | stock input   | price diff   | Inventory Value   | Qty On Hand   | Avg Cost   |
+=========================================+===============+==============+===================+===============+============+
|                                         |               |              | $0                | 0             | $0         |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Receive 8 tables at $10                 | ($80)         |              | $80               | 8             | $10        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Receive vendor bill $80                 | $0            |              | $80               | 8             | $10        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Receive 4 tables at $16                 | ($64)         |              | $144              | 12            | $12        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Receive vendor bill $64                 | $0            |              | $144              | 12            | $12        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Deliver 10 tables to customer           | $0            |              | $24               | 2             | $12        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Return 1 table initially bought at $10  | **$10**       | **$2**       | **$12**           | 1             | $12        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+
| Receive vendor refund $10               | $0            | $2           | $12               | 1             | $12        |
+-----------------------------------------+---------------+--------------+-------------------+---------------+------------+


Explaining journal entries above
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. When 8 tables arrive at the warehouse, :ref:`AVCO inventory valuation
   <inventory/avg_cost/math_table>` is calculated on the incoming products.

   #. Inventory value increases by `$80`, as it reflects that the company currently has `$80` worth
      of tables in stock.
   #. `$80` is set aside in the *stock input* account, since this amount is owed to the vendor.
   #. Credit the *stock input* account `$80`.
   #. Debit the *Inventory Valuation* account `$80`.

#. Bill sent by vendor for the 8 tables is received.

   #. Use `$80` in the *stock input* account to pay the bill. This cancels out and the account now
      holds `$0`.
   #. Debit the *stock input* account `$80`.
   #. Credit the *Account payable* `$80`. This account stores the amount the company owes others, so
      accountants use the amount to write checks to vendors.

#. When 4 tables arrive at the warehouse, the inventory valuation is recalculated using the methods
   explained :ref:`previously<inventory/avg_cost/math_table>`.

   #. The *stock input* account stores the `$64` owed to the vendor. The amount in this account is
      unrelated to the inventory value, because the amount the table is valued at within the company
      is separate from the price the vendor sold the product for.
   #. Credit *stock input* account `$64`.

#. Similarly to #2, when the bill for 4 tables is received,
   #. *Stock input* account is debited, which reconciles it to `$0`.
   #. On the opposite end, *Account payable* is credited, and the `$64` is handed off to the
      accountants to pay the vendor bill.

#. When 10 products are delivered, the *stock input* account is untouched because there are no new
   products coming in.
   #. Inventory valuation credited `$120`

#. Return 1 table to the vendor who sold it at `$10`
   #. Debit *stock input* account `$12` (Why?)
      #. also how to account for the `$2` price difference?
   #. Credit *stock valuation* `$12` because the item is leaving the stock

#. Receive vendor refund
   #. Credit *stock input* account `$10`
   #. Debit *Account payable* `$10` to have the accountants collect and register the payment in their
      journal.
