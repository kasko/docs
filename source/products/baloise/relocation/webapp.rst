Relocation Web App
===================

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "Relocation"
   "Insurer", "Baloise"
   "ID DE", "zG13lWZ8KQykqaXwZPDNPAb6w7VOYdve"
   "ID EN", "n5l3L4wzQEPJykxvnjxqaBVZR78dvj0o"
   "ID FR", "4AzgY2WZk1eNaRM870x98r5vP6Jlj0oQ"
   "Language", "DE|FR|EN"
   "Region", "Switzerland"

Kasko JS Required Data Parameters
---------------------------------
Prefill data must be defined in the "data" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "salutation",          "Allowed values: mr, ms",                       "mr"
   "first_name",          "Free text string up to 255 characters.",       "John"
   "last_name",           "Free text string up to 255 characters.",       "Doe"
   "email",               "A valid email address.",                       "john@example.com"
   "phone",               "Free text string up to 255 characters.",       "2222222222"
   "dob",                 "Optional date of birth. Format: YYYY-MM-DD",   "1990-07-21"
   "current_address",     "Free text string up to 255 characters.",       "Main Street 5"
   "current_postcode",    "Free text string up to 255 characters.",       "W1U6BT"
   "current_city",        "Free text string up to 255 characters.",       "London"
   "relocation_address",  "Free text string up to 255 characters.",       "Secondary Street 12b"
   "relocation_postcode", "Free text string up to 255 characters.",       "W1U6BT"
   "relocation_city",     "Free text string up to 255 characters.",       "London"
   "relocation_date",     "Relocation (moving) date. Format: YYYY-MM-DD", "2017-07-21"
   "relocation_price",    "Relocation (moving) price in cents.",          "200000"
   "bad_frog_purchased",  "Weather BAD FROG has been purchased. Boolean", "true"
   "all_risk_purchased",  "Weather ALL RISK has been purchased. Boolean", "false"

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
        data: {
          salutation: "mr",
          first_name: "John",
          last_name: "Doe",
          email: "john@example.com",
          phone: "2222222222",
          dob: "1990-07-21",
          current_address: "Main Street 5",
          current_postcode: "W1U6BT",
          current_city: "London",
          relocation_address: "Secondary Street 12b",
          relocation_postcode: "W1U6BT",
          relocation_city: "London",
          relocation_date: "21-07-1990",
          relocation_price: 200000,
          bad_frog_purchased: true,
          all_risk_purchased: false
        },
        metadata: {
          salesagentid: 123456789
        }
      });
    </script>
