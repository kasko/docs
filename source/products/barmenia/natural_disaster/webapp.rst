Webapp
=======

Kasko JS Required Data Parameters
---------------------------------
Optional data that can be put in the "data" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "theme", "Optional parameter. Allowed values: barmenia-direkt"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "j6NWlb4k1Lmq2PXZK6xJEov3QZ0peOyw",
        mode: "fullscreen",
        theme: "barmenia-direkt",
        metadata: {
          agent_id: "123456789"
        }
      });
    </script>
