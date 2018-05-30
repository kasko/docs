Webapp
======

Kasko JS data Parameters
-------------------------------------
Optional properties that can be put in the "data" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "first_name",          "Preselected invited user first name",        "Wolf"
   "email",               "Preselected invited user email",             "email@email.com"
   "invited_by",          "Name of person who invited to join group",   "Max Mustermann"
   "group_id",            "Flatshare Group ID to join",                 "WG-12345678"
   "house_number",        "Preselected invited user house number",      "123"
   "street",              "Preselected invited user street",            "street"
   "city",                "Preselected invited user city",              "city"
   "postcode",            "Preselected invited user postcode",          "12345"
   "housemates_count",    "Housemates count in group",                  2

Kasko JS config Parameters
-------------------------------------
Optional data that can be put in the "config" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "portal_button_url",  "Webapp's back button URL.",      "https://www.kasko.io/"

Kasko JS Optional metadata Parameters
-------------------------------------
Optional data that can be put in the "metadadata" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "salesagent_id",    "The ID of the sales agent.",     "123456789"
   "subagent_id",      "The ID of the sub-agent.",       "123456789"
   "salesagent_email", "The BCC salesagent email.",      "test@email.com"
   "subagent_email",   "The BCC sub-agent email.",       "test@email.com"

Example Kasko JS embed code for New Client
------------------------------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint : "tp_fa87ed4d89c7dbdad287884237ff4",
        mode: "fullscreen",
        language: "de",
        config: {
            back_button_url: "https://www.kasko.io/"
        },
        metadata: {
            salesagent_id: "123456789",
            subagent_id: "123456789",
            salesagent_email: "test@email.com",
            subagent_email: "test@email.com"
        }
      });
    </script>

Example Kasko JS embed code for Joining Group
---------------------------------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint : "tp_1c858d8f2558ee84e073633330f01",
        mode: "fullscreen",
        language: "de",
        config: {
            portal_button_url: "https://www.kasko.io/"
        },
        data: {
            first_name: "Wolf",
            email: "email@email.io",
            invited_by: "Max Mustermann",
            group_id: "WG-12345678",
            house_number: 123,
            street: "street",
            city: "City",
            postcode: "1234",
            housemates_count: 2
        },
        metadata: {
            salesagent_id: "123456789",
            subagent_id: "123456789",
            salesagent_email: "test@email.com",
            subagent_email: "test@email.com"
        }
      });
    </script>
