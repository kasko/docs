Webapp
======

KASKO JS Data Parameters
------------------------

Optional data that can be put in the ``data`` tag of the KASKO embed object.

.. csv-table::
   :header: "Name", "Required", "Type", "Description", "Example Value"

   "product",       "``no``", "`string`", "Product",           "``1`` - Bicycle, ``2`` - Electric bicycle"
   "product_price", "``no``", "`string`", "Price in CHF",      "500"
   "duration",      "``no``", "`string`", "Duration in years", "2"

Example KASKO JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "d7zoBRlEp9yar6XyQBXPWm05VqwkQKA8",
        overlay_color: "#FFFFFF",
        data: {
          quote: {
            product: "2",
            product_price: "500",
            duration: "2"
          }
        },
        metadata: {
          salesagent_id: "123456789"
        }
      });
    </script>
