oBike Insurance
===============

Product Information
^^^^^^^^^^^^^^^^^^^

.. csv-table::
   :widths: 25, 75

   "Name",      "Baloise oBike"
   "Insurer",   "Baloise Switzerland"
   "Region",    "Switzerland"
   "Languages", "``DE``, ``FR``, ``IT``, ``EN``"

Product IDs
^^^^^^^^^^^

.. csv-table::
   :widths: 20, 40, 40
   :header: "Type (language)", "Product ID", "Variant ID"

   "New customer (DE)", "``9znERVLG5Wwk3dXdkBDBPyJ4vQrpAb0e``", "``2ekYGz8ProWy1BMV6lMjAd6nVp9NL5vb``"
   "New customer (FR)", "``d2va8Owp4WRnkbMpqkDAmVoN1JejB5GE``", "``VW9AmkbvRBqZN4xoqnDlLgEOry13djnK``"
   "New customer (IT)", "``pJeQgLmKblzBR6x5PKX5vdqEkV3wP7an``", "``9znERVLG5Wwk3dXdLPXBPyJ4vQrpAb0e``"
   "New customer (EN)", "``VW9AmkbvRBqZN4xoroXlLgEOry13djnK``", "``4AzgY2WZk1eNaRM82aX98r5vP6Jlj0oQ``"

   "Renewal (DE)", "``RQA8lkzNvBLb0pDbV1DyP16Go3Wwe4qn``", "``KP8VEOzroaqw15DnEkMyd64JZ2jG7egk``"
   "Renewal (FR)", "``d7zoBRlEp9yar6XyrjxPWm05VqwkQKA8``", "``d2leL7QmGaOyz4M3lvDPgrkVBEqK6RJZ``"
   "Renewal (IT)", "``LPE5mgGpndOQZoX741xze0w8aK7Br3N1``", "``RQA8lkzNvBLb0pDbmWMyP16Go3Wwe4qn``"
   "Renewal (EN)", "``d2leL7QmGaOyz4M3JjxPgrkVBEqK6RJZ``", "``n5l3L4wzQEPJykxvWQMqaBVZR78dvj0o``"

Integration methods
^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1

   rest-api


REST API
========

Get Quote Request
-----------------

Data fields
~~~~~~~~~~~

The language of the policy docs & email is set with variant_id (see table above). For first time customers use "New Customer (XX)" variant_id. If the customer has purchased insurance before, please use the variant ID's from "renewal". No additional data will have to be appended to the quote request.


Example Request
~~~~~~~~~~~~~~~

Below is an example request for a new customer (English). 

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u sk_test_V6oM7OYsBWdR7AWD34QnFDXBsIaskG2T: \
        -d variant_id=4AzgY2WZk1eNaRM82aX98r5vP6Jlj0oQ 


Policy Data Fields
------------------

Create Unpaid Policy Request
----------------------------

Data fields
~~~~~~~~~~~

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "domicile",        "yes",         "``string``", "The country where the customer is currently domiciled"
   "phone",           "yes",         "``string``", "The mobile phone number of the customer"
   "dob",             "yes",         "``string``", "Customers date of birth in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD)."


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
                "domicile": "Germany",
                "phone": "0123456789",
                "dob": "1980-12-31"
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
