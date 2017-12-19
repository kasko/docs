Webapp
======

Kasko JS Optional parameters
-------------------------------------
Optional data that can be put in the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "tags",  "An array of item tags that will be exposed in the webapp. If this property is not provided - all items will be exposed.", "['camera', 'watch']"


Kasko JS Optional metadata Parameters
-------------------------------------
Optional data that can be put in the "metadadata" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "back_button_url", "Webapp's back button URL.",      "https://www.kasko.io/"
   "agent_id",        "The ID of the sales agent.",     "123456789"
   "sub_agent_id",    "The ID of the sales sub-agent.", "123456789"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint : "tp_e46a5ac3b8eee88c49f3a0cb222a2",
        mode: "fullscreen",
        language: "de",
        metadata: {
          back_button_url: "https://www.kasko.io/",
          agent_id: "123456789",
          sub_agent_id: "123456789",
          bcc_email: "test@email.com"
        }
      });
    </script>

Example Kasko JS embed code with filtered item list
---------------------------------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint : "tp_e46a5ac3b8eee88c49f3a0cb222a2",
        mode: "fullscreen",
        language: "de",
        tags: ["camera", "watch"]
      });
    </script>
