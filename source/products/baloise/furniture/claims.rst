Claims App
==========

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
        baseURL: "https://claims.kasko.io",
        key: "KEY",
        product : "LJROpwloaQ8ZBmMA93M7W5PyE4qvAb31",
        mode: "fullscreen",
        metadata: {
          salesagentid: 123456789
        }
      });
    </script>
