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

   "luggage_insurance",  "yes", "``boolean``", "Luggage Cover, accepted values are ``true`` for ``false``."
    "luggage_value",  "``yes``", "`int`",  "The value of luggage. Increments of ``1000`` starting from ``0``, up to ``20000``."
   "reduced_excess",  "yes", "``boolean``", "Reduced Excess Cover, accepted values are ``true`` for ``false``."
   "camper",  "yes", "``boolean``", "Reduced Excess is on camper, accepted values are ``true`` for ``false``."
   "trip_start_date",  "yes", "``date``", "Trip start date, yyyy-mm-dd (2018-06-20)."
   "trip_end_date",  "yes", "``date``", "Trip end date, , yyyy-mm-dd (2018-07-01)."


Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=2ekYGz8ProWy1BMVNrMjAd6nVp9NL5vb \
        -d data='{"luggage_insurance":true,"luggage_value":500000,"reduced_excess":true,"trip_end_date":"2018-06-20","trip_start_date":"2018-06-13","camper":false}'

Create Unpaid Policy Request
----------------------------

Data fields
^^^^^^^^^^^

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "salutation",      "yes", "``string``", "Customers gender, accepted values are ``mr`` or ``ms``."
   "dob",         "yes", "``string``", "Customers date of birth in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD)."
   "phone",       "yes", "``string``", "Customers phone number."
   "house_number", "yes", "``string``", "Customers house number."
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
                "salutation": "mr",
                "dob": "1990-12-31",
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
