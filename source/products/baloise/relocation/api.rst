Relocation API
===================

**Please remember to use the API specific variant of a product.**

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


Quote Data
----------
Query string data appended to the quote request

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "relocation_price",            "int",    "Relocation (moving) price in cents.",                                  "200000"
   "relocation_date",             "string", "Relocation (moving) date. Format: YYYY-MM-DD",                         "2017-07-21"
   "bad_frog_insurance",          "bool",   "Weather the customer wants to buy BAD FROG insurance.",                "true"
   "all_risk_insurance",          "bool",   "Weather the customer wants to buy ALL RISK insurance.",                "false"
   "contents_cover_duration",     "string", "Contents cover duration. Available values: P0Y, P1Y, P2Y",             "P0Y"
   "mobile_phone_cover_duration", "string", "Mobile phone cover (handy) duration. Available values: P0Y, P1Y, P2Y", "P1Y"


Policy Data
-----------
JSON data posted to /policies on creation of policy

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "current_address",     "string", "Free text string up to 255 characters.",     "Main Street 5"
   "current_postcode",    "string", "Free text string up to 255 characters.",     "W1U6BT"
   "current_city",        "string", "Free text string up to 255 characters.",     "London"
   "relocation_address",  "string", "Free text string up to 255 characters.",     "Secondary Street 12b"
   "relocation_postcode", "string", "Free text string up to 255 characters.",     "W1U6BT"
   "relocation_city",     "string", "Free text string up to 255 characters.",     "London"
   "phone",               "string", "Free text string up to 255 characters.",     "2222222222"
   "dob",                 "string", "Optional date of birth. Format: YYYY-MM-DD", "1990-07-21"
   "salutation",          "string", "Allowed values: mr, ms",                     "mr"
