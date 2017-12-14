REST API
========

**Please remember to use the API specific item of a touchpoint.**

Quote Data
----------
Query string data appended to the quote request

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "duration",                "str",   "Insurance duration. Available values (depending on item): P1Y, P2Y, P3Y, P4Y, P5Y", "P1Y"
   "item_value",              "int",   "Item value in cents.", "200000"
   "older_than_three_months", "bool",  "Has the item been purchased in the last 3 months?", "false"
   "theft",                   "bool",  "Theft module.", "true"
   "damage",                  "bool",  "Damage module.", "false"
   "warranty",                "bool",  "Warranty module (not available for all items).", "false"
   "loss",                    "bool",  "Loss module (can be set to `true` only if theft is also `true`).", "false"
   "item_purchase_date",      "str",   "Item purchase date. Required if `warranty` module is enabled.", "2017-10-04"
   "item_manufacturers_warranty_duration", "str", "Item manufacturers warranty duration. Required if `warranty` module is enabled. Available values: P1Y, P2Y ... P9Y, P10Y.", "P1Y"


Policy Data
-----------
JSON data posted to /policies on creation of policy

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "phone",             "string", "Free text string up to 255 characters.",      "+417304200"
   "salutation",        "string", "Customer title. Available values: mr, ms.",   "mr"
   "dob",               "string", "Date of birth of the policholder.",           "1989-02-04"
   "house_number",      "string", "House number of the policyholder's address.", "12"
   "street",            "string", "Street name of the policyholder's address.",  "Main street"
   "city",              "string", "City of the policyholder's address.",         "Lubeck"
   "postcode",          "string", "Postcode of the policyholder's address.",     "1234"
   "item_identifier",   "string", "Unique item identifier (receipt number, IMEI etc.)", "WT3211"
   "item_description",  "string", "Description of the item (i.e. make + model).", "Nikon D3000"
