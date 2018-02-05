Webapp
======

Kasko JS Optional ``metadata`` Parameters
-----------------------------------------

Optional data that can be put in the ``metadadata`` tag of the kasko embed object.

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "salesagent_id", "The ID of the sales agent.", "``123456789``"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        product : "g9rVJqB3lE15zoXLmJxK8kbp0RmZOW6Q",
        mode: "fullscreen",
        metadata: {
          salesagent_id: "123456789"
        }
      });
    </script>
