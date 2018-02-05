Baloise Watch (Distributor) Insurance
=====================================

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "Watch Distributor"
   "Insurer", "Baloise"
   "ID DE", "g9rVJqB3lE15zoXLNkMK8kbp0RmZOW6Q"
   "ID IT", "LJROpwloaQ8ZBmMARnD7W5PyE4qvAb31"
   "ID FR", "WKLe03JjQRwmZzDYyjD7Ob4NPlk8rpyv"
   "Language", "DE|FR|IT"
   "Region", "Switzerland"

Kasko JS Optional metadata Parameters
-------------------------------------
Optional data that can be put in the "metadadata" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "salesagent_id",  "The ID of the sales agent.", "123456789"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "g9rVJqB3lE15zoXLNkMK8kbp0RmZOW6Q",
        mode: "fullscreen",
        data: {
          luggage_value: 3000,
          reduced_excess: true
        },
        metadata: {
          salesagent_id: 123456789
        }
      });
    </script>
