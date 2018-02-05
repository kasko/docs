Watch
===================

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "Watch"
   "Insurer", "Basler"
   "DE", "KJAR5gL1GY0j6QDGA4x72z3v9yoO8VkB"
   "Language", "DE"
   "Region", "Germany"

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
        product : "KJAR5gL1GY0j6QDGA4x72z3v9yoO8VkB",
        mode: "fullscreen",
        metadata: {
          salesagent_id: 123456789
        }
      });
    </script>
