REST API
========

Get Quote Request
-----------------

Data fields
^^^^^^^^^^^

Query string data appended to the quote request

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "product",  "yes", "``string``", "Risk module, accepted values are ``product_a``, ``product_b`` and ``product_c``."
   "addon_1",  "yes", "``boolean``", "Indiciated wether addon 1 is used. Accepted values are ``true`` or ``false``."
   "insured_circle",  "yes", "``string``", "Insured Circle, accepted values are ``circle_a``, ``circle_b``."
   "age_group",  "if insured_circle = circle_a", "``string``", "Aged group if circle a, accepted values are ``under_30``, ``between_30_and_50``, ``above_50``."
   "share_data",  "yes", "``boolean``", "Indiciated wether share data is used. Accepted values are ``true`` or ``false``."
   "type_of_item",  "yes", "``string``", "Item Class, accepted values are ``item_a``, ``item_a_and_b``, ``item_a_and_b_and_c``."
   "deductible",  "yes", "``integer``", "Deductable, accepted values are ``0``, ``20000``."
  "cancellation_window",  "yes", "``string``", "The allowed cancellation window, accepted values are ``day``, ``month``, ``year``."

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=d2leL7QmGaOyz4M35rXPgrkVBEqK6RJZ \
        -d data='{"age_group":"under_30","type_of_item":"item_a_and_b","share_data":true,"product":"product_a","addon_1":true,"insured_circle":"circle_b","cancellation_window":"month","deductible":20000}'

Create Unpaid Policy Request
----------------------------

Data fields
^^^^^^^^^^^

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "gender",      "yes", "``string``", "Customers gender, accepted values are ``male`` or ``female``."
   "dob",         "yes", "``string``", "Customers date of birth in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD)."
   "phone",       "yes", "``string``", "Customers phone number."
   "home_number", "yes", "``string``", "Customers house number."
   "street",      "yes", "``string``", "Customers street."
   "city",        "yes", "``string``", "Customers city."
   "postcode",    "yes", "``string``", "Customers postal code, must be a valid Swiss postal code."

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/policies \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "quote_token": "<TOKEN OBTAINED FORM QUOTE REQUEST>",
            "first_name": "FirstName",
            "last_name": "LastName",
            "email": "test@kasko.io",
            "data": {
                "gender": "female",
                "dob": "1990-12-31",
                "phone": "+41781234567",
                "home_number": "1",
                "street": "Street",
                "city": "City",
                "postcode": "1234"
            }
        }'

Convert Policy To Paid Request
------------------------------

After creating unpaid policy it is required to convert it to paid. This can be done by making another request.

Data fields
^^^^^^^^^^^

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``payment_token`` returned by the create policy request."
   "policy_id", "yes", "``string``", "The 33 character long policy ID returned by the create policy request."

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>"
        }'
