oBike new customer policy API
===================

**Please remember to use the API specific variant of a product.**

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "oBike New customer"
   "Insurer", "Baloise"
   "Language", "DE|FR|IT|EN"
   "Region", "Switzerland"

Product IDs
-----------

.. csv-table::
   :widths: 20, 40, 40
   :header: "Language", "Product ID", "Variant ID"

   "``DE``", "``9znERVLG5Wwk3dXdkBDBPyJ4vQrpAb0e``", "``2ekYGz8ProWy1BMV6lMjAd6nVp9NL5vb``"
   "``FR``", "``d2va8Owp4WRnkbMpqkDAmVoN1JejB5GE``", "``VW9AmkbvRBqZN4xoqnDlLgEOry13djnK``"
   "``IT``", "``pJeQgLmKblzBR6x5PKX5vdqEkV3wP7an``", "``9znERVLG5Wwk3dXdLPXBPyJ4vQrpAb0e``"
   "``EN``", "``VW9AmkbvRBqZN4xoroXlLgEOry13djnK``", "``4AzgY2WZk1eNaRM82aX98r5vP6Jlj0oQ``"

Policy Data
-----------
JSON data posted to /policies on creation of policy

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "phone",    "string", "Free text string up to 255 characters.", "000000000"
   "domicile", "string", "Free text string up to 255 characters.", "Switzerland"
