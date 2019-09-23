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

   "purchase_date",   "yes",   "``string``",    "Date in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format. Max allowed date: ``today``."
   "purchase_value",  "yes",   "``integer``",   "Purchase value in cents."
   "duration",        "yes",   "``string``",    "Policy duration in years. Allowed values: ``P5Y`` for ``5 year`` duration."


Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/quotes \
        -u <YOUR SECRET API KEY>: \
        -d variant_id=LPE5mgGpndOQZoX7WbXze0w8aK7Br3N1 \
        -d data='{"purchase_date":"2017-09-05","purchase_value":"120000","duration":"P5Y"}'

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
   "postcode",       "yes",   "``string``",  "Customers postal code, must be a valid Swiss postal code."
   "receipt_number", "yes",   "``string``",  "Receipt number of the purchased item(s)."
   "payee_private",  "yes",   "``string``",  "Flag identifying flow. Available values: private,company."
   "company_industry",       "yes",   "``string``",  "Company industry name. Has to be set if payee_private is company."
   "company_legal_form",       "yes",   "``string``",  "Company legal form number. Has to be set if `payee_private` is `company`."
   "company_name",       "yes",   "``string``",  "Name of the company. Has to be set if `payee_private` is `company`."
   "company_uid",       "no",   "``string``",  "Company unique identifier."
   "crefo_uid",       "no",   "``string``",  "Credit reform unique identifier."
   

   

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
                "postcode": "1234",
                "payee_private": "company",
                "company_industry": "Architekturbüros",
                "company_legal_form: "10",
                "company_name": "Kasko",
                "company_uid": "CH00000000000",
                "crefo_uid": "CHE-000.000.000",
                "receipt_number": "REF123"
            }
        }'
