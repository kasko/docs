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
 

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=d2leL7QmGaOyz4M35rXPgrkVBEqK6RJZ \
        -d data=' {"age_group":"between_30_and_50","type_of_item":"item_a_and_b","share_data":true,"product":"product_a","addon_1":true,"insured_circle":"circle_a","cancellation_window":"month","deductible":20000}'

Create Unpaid Policy Request
----------------------------

Data fields
^^^^^^^^^^^

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "salutation",     "yes",   "``string``",  "Customers salutation, accepted values are ``mr`` or ``ms``."
   "dob",            "yes",   "``string``",  "Customers date of birth in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD)."
   "phone",          "yes",   "``string``",  "Customers phone number."
   "house_number",   "yes",   "``string``",  "Customers house number."
   "street",         "yes",   "``string``",  "Customers street."
   "city",           "yes",   "``string``",  "Customers city."
   "postcode",       "yes",   "``string``",  "Customers postal code"

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
                "salutation": "mr",
                "dob": "1990-12-12",
                "phone": "+41781234567",
                "house_number": "1",
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
