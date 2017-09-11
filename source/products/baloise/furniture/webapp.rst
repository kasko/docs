Webapp
=======

Kasko JS Optional metadata Parameters
-------------------------------------
Optional data that can be put in the "metadadata" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "salesagentid",  "The ID of the sales agent.", "123456789"

Example Kasko JS embed code
---------------------------
.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "zG13lWZ8KQykqaXwZPDNPAb6w7VOYdve",
        mode: "fullscreen",
        metadata: {
          salesagentid: 123456789
        }
      });
    </script>
