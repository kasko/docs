REST API
========

Get Quote Request
-----------------

Data fields
~~~~~~~~~~~

Query string data appended to the quote request

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "watch_value",     "yes", "``int``",    "Watch value in CHF cents, the value must be an integer between ``20000`` or ``3000000``."
   "policy_duration", "yes", "``string``", "The duration of the policy, accepted values are ``P1Y`` (1 year), ``P2Y`` (2 years) or ``P3Y`` (3 years)."

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=pJeQgLmKblzBR6x5PKX5vdqEkV3wP7an \
        -d data='{"watch_value":20000,"policy_duration":"P1Y"}'

Create Unpaid Policy Request
----------------------------

Data fields
~~~~~~~~~~~

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "watch_make",   "yes", "``string``", "Watch make (manufacturer)."
   "watch_model",  "yes", "``string``", "Watch model."
   "salutation",   "yes", "``string``", "Customers salutation, accepted values are ``mr`` or ``ms``."
   "dob",          "yes", "``string``", "Customers date of birth in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD)."
   "phone",        "yes", "``string``", "Customers phone number."
   "house_number", "yes", "``string``", "Customers house number."
   "street",       "yes", "``string``", "Customers street."
   "city",         "yes", "``string``", "Customers city."
   "postcode",     "yes", "``string``", "Customers postal code, must be a valid German postal code."

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/policies \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "quote_token": "<TOKEN OBTAINED FROM QUOTE REQUEST>",
            "first_name": "FirstName",
            "last_name": "LastName",
            "email": "test@kasko.io",
            "data": {
                "watch_make": "Rolex",
                "watch_model": "Daytona",
                "salutation": "mr",
                "dob": "1990-12-31",
                "phone": "+49000000000",
                "house_number": "1",
                "street": "Street",
                "city": "City",
                "postcode": "12345"
            }
        }'

Convert Policy To Paid Request
------------------------------

After creating unpaid policy it is required to convert it to paid. This can be done by making another request.

Data fields
~~~~~~~~~~~

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``",  "The ``payment_token`` returned by the create policy request."
   "policy_id", "yes", "``string``",  "The 33 character long policy ID returned by the create policy request."

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>"
        }'
