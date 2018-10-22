REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

V2 Header
----------

The API requests must use the V2 header as show in the examples below:

``Accept: application/vnd.kasko.v2+json``

Quote Data
----------
Query string data appended to the quote request

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "duration",                "str",   "Insurance duration. Available values (depending on item): P1Y"
   "start_date",              "iso_date",   "Start date of policy  ISO date", "yyyy-mm-dd"
   "main_module",             "str",  "Which insurance module", "PBV|PBVIM"
   "partner_coverage",        "bool",  "Coverage for partner", "true|false"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json'
       -d touchpoint_id=TOUCHPOINT_ID \
       -d item_id=item_b22001a5cb67ddc81ce7db58647 \
       -d subscription_plan_id=sp_01a1cc12c98ccc5b2c7a2d1e9fa09 \
       -d data='{"duration":"P1Y","start_date":2018-12-12,"main_module":PBV,"partner_coverage":true}'

Create Unpaid Policy Request
----------------------------
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "phone",             "string", "Free text string up to 255 characters.",      "+417304200"
   "salutation",        "string", "Customer title. Available values: mr, ms.",   "mr"
   "dob",               "string", "Date of birth of the policholder.",           "1989-02-04"
   "house_number",      "string", "House number of the policyholder's address.", "12"
   "street",            "string", "Street name of the policyholder's address.",  "Main street"
   "state",              "string", "State of the policyholder's address.",         "State"
   "postcode",          "string", "Postcode of the policyholder's address.","1234"
   "employment_status",  "string", "Employment status",     "employed"
   "coinsured_first_name", "string|optional", "Co-insured First name","test"
   "coinsured_first_name", "string|optional", "Co-insured Last name", "person"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

	curl -X POST \
	  'https://api.kasko.io/policies' \
	  -u sk_test_SECRET_KEY: \
	  -H 'Accept: application/vnd.kasko.v2+json' \
	  -H 'Content-Type: application/json' \
	  -d '{
	  "data": {
			"phone":"+11111",
			"salutation": "mr",
			"dob": "1991-10-31",
			"street": "Test Street",
			"state": "Test State",
			"postcode": "1001",
			"employment_status": "employed"
	  },
	  "quote_token":"<Quote Token>",
	  "first_name": "Test",
	  "last_name": "Person",
	  "email": "test@kasko.io",
	  "language": "de"
        }'

Convert Policy To Paid Request
------------------------------
After creating unpiad policy it is required to convert it to paid. This can be done by making another request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``payment_token`` returned by the create unpaid policy request."
   "policy_id", "yes", "``string``", "The 33 character long policy ID returned by the create unpaid policy request."
   "method", "yes", "``string``", "Payment method ``distributor``."
   "provider", "yes", "``string``", "Payment provider ``distributor``."
 

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>",
            "method": "distributor",
            "provider": "distributor",
        }'
