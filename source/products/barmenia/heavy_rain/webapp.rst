Webapp
=======

Kasko JS Required Data Parameters
---------------------------------
Optional data that can be put in the "data" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "cover", "Allowed values: 100000,500000,1000000", "100000"
   "theme", "Optional parameter. Allowed values: barmenia-direkt"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "kY1R5y98qPZJK7xRk7MWmrgvz6dpBLEN",
        mode: "fullscreen",
        theme: "barmenia-direkt",
        data: {
          cover: 100000,
        },
        metadata: {
          salesagentid: "123456789"
        }
      });
    </script>
