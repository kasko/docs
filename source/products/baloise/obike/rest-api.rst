REST API
========

**Please remember to use the specific variant of a product (new customer vs renewal).**

Get Quote Request
-----------------

``data`` attribute of the quote request must be empty.

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=4AzgY2WZk1eNaRM82aX98r5vP6Jlj0oQ \
        -d data='{}'

Policy Data Fields
------------------

Create Unpaid Policy Request
----------------------------

Data fields
~~~~~~~~~~~

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "phone", "yes", "``string``", "Phone number. Free text string up to 255 characters."
   "domicile", "yes", "``string``", "Place of residence. Free text string up to 255 characters."
   "dob", "yes", "``string``", "Date of birth in ISO date format (YYYY-MM-DD)."

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
                "phone": "2222222222"
                "domicile": "Switzerland"
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
