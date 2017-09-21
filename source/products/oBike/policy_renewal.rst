oBike customer policy renewal API
===================

**Please remember to use the API specific variant of a product.**

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "oBike Renewal"
   "Insurer", "Baloise"
   "Language", "DE|FR|IT|EN"
   "Region", "Switzerland"

Product IDs
-----------

.. csv-table::
   :widths: 20, 40, 40
   :header: "Language", "Product ID", "Variant ID"

   "``DE``", "``RQA8lkzNvBLb0pDbV1DyP16Go3Wwe4qn``", "``KP8VEOzroaqw15DnEkMyd64JZ2jG7egk``"
   "``FR``", "``d7zoBRlEp9yar6XyrjxPWm05VqwkQKA8``", "``d2leL7QmGaOyz4M3lvDPgrkVBEqK6RJZ``"
   "``IT``", "``LPE5mgGpndOQZoX741xze0w8aK7Br3N1``", "``RQA8lkzNvBLb0pDbmWMyP16Go3Wwe4qn``"
   "``EN``", "``d2leL7QmGaOyz4M3JjxPgrkVBEqK6RJZ``", "``n5l3L4wzQEPJykxvWQMqaBVZR78dvj0o``"

Policy Data
-----------
JSON data posted to /policies on creation of policy

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "phone",    "string", "Free text string up to 255 characters.", "000000000"
   "domicile", "string", "Free text string up to 255 characters.", "Switzerland"
