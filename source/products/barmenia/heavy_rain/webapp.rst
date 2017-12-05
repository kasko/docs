Webapp
=======

Kasko JS Required Data Parameters
---------------------------------
Optional data that can be put in the "data" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "cover", "Allowed values: 100000,500000,1000000", "100000"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "kY1R5y98qPZJK7xRk7MWmrgvz6dpBLEN",
        mode: "fullscreen",
        data: {
          cover: 100000,
        }
      });
    </script>
