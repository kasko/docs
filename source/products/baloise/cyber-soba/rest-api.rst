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

   "product",  "yes", "``string``", "Risk module, accepted values are ``1`` for ``Safe Pay``, ``2`` for ``Safe Surf`` and ``3`` for ``Safe Pay & Safe Surf``."
   "family",   "yes", "``string``", "Indiciated wether it will be a single or famility cover. Accepted values are ``1`` for ``Single`` and ``2`` for ``Family``."
   "duration", "yes", "``string``", "The policy duration in years, accepted values are ``P1Y`` for ``single year``, ``P2Y`` for ``two years`` and ``P3Y`` for ``three years``."

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=9znERVLG5Wwk3dXdR3MBPyJ4vQrpAb0e \
        -d data='{"duration":"1","product":"3","family":"2"}'

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
