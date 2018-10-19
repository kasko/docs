REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

V2 Header
----------

The API requests must use the V2 header as show in the examples below

Quote Data
----------
Query string data appended to the quote request

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "duration",                "str",   "Insurance duration. Available values (depending on item): P1Y"
   "start_date",              "iso_date",   "Start date of policy.  ISO date", "yyyy-mm-dd"
   "main_module",             "str",  "Which insurance module", "PBV|PBVIM"
   "marital_status",           "str",  "Customer marital status", "single|married"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -d touchpoint_id=TOUCHPOINT_ID \
       -d language=de \
       -d data='{"duration":"P1Y","item_value":100000,"older_than_three_months":false,"theft":true,"damage":false,"loss":false}'

Policy Data
-----------
JSON data posted to /policies on creation of policy

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "phone",             "string", "Free text string up to 255 characters.",      "+417304200"
   "salutation",        "string", "Customer title. Available values: mr, ms.",   "mr"
   "dob",               "string", "Date of birth of the policholder.",           "1989-02-04"
   "house_number",      "string", "House number of the policyholder's address.", "12"
   "street",            "string", "Street name of the policyholder's address.",  "Main street"
   "city",              "string", "City of the policyholder's address.",         "Lubeck"
   "postcode",          "string", "Postcode of the policyholder's address.",     "1234"
   "item_identifier",   "string", "Unique item identifier (receipt number, IMEI etc.)", "WT3211"
   "item_description",  "string", "Description of the item (i.e. make + model).", "Nikon D3000"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl https://api.kasko.io/policies \
       -u sk_test_SECRET_KEY: \
       -d quote_token=QUOTE_TOKEN \
       -d first_name=John \
       -d last_name=Doe \
       -d email=john@example.com \
       -d language=de \
       -d data='{"phone":"+417304200","salutation":"mr","dob":"1989-02-04","house_number":"12","street":"Main street","city":"Lubeck","postcode":"1234","item_identifier":"WT3211","item_description":"Nikon D3000"}'

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

