REST API
========

.. info::
  The language of the policy docs & email is set with ``variant_id``.
  For first time customers use "New Customer (XX)" ``variant_id``.
  If the customer has purchased insurance before, please use the variant ID's from "renewal".
  No additional data will have to be appended to the quote request.

Get Quote Request
-----------------

Data fields
~~~~~~~~~~~

For this product ``data`` attribute of the quote request has not fields and must be empty.

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=4AzgY2WZk1eNaRM82aX98r5vP6Jlj0oQ \
        -d data='{}'

Create Unpaid Policy Request
----------------------------

Data fields
~~~~~~~~~~~

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "phone",    "yes", "``string``", "The mobile phone number of the customer."
   "domicile", "yes", "``string``", "The country where the customer is currently domiciled."
   "dob",      "yes", "``string``", "Customers date of birth in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD)."

Example Request
~~~~~~~~~~~~~~~

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
                "phone": "0123456789",
                "domicile": "Germany",
                "dob": "1988-08-22"
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

   "token",     "yes", "``string``", "The ``payment_token`` returned by the create policy request."
   "policy_id", "yes", "``string``", "The 33 character long policy ID returned by the create policy request."

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
