REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint .**

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

   "housemates_count",        "integer",   "Number of house members, always 1", "1"
   "items_count",             "integer",   "Number of items insured", "1"
   "car_sharing",             "bool",      "Whether or not car sharing selected", "false"
   "start_date",              "iso_date",  "Start date of policy  ISO date", "yyyy-mm-dd"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -d '{
        "key": "<PUBLIC KEY>",
        "language": "de",
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID>",
        "subscription_plan_id": "<SUBSCRIPTION PLAN ID>",
        "data": {
            "housemates_count": 1,
            "items_count": 3,
            "car_sharing": false,
            "start_date": "2018-12-13",
            "skip_items": false
        }
    }'

Create Unpaid Policy Request
----------------------------
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 35, 20, 75, 20

   "phone",                           "string",   "Free text string up to 255 characters.",   "+411111111"
   "house_number",                    "string",   "House number of the policyholder's address.",   "12"
   "street",                          "string",   "Street name of the policyholder's address.",   "Main street"
   "city",                            "string",   "City name.",  "London"
   "postcode",                        "string",   "Postcode of the policyholder's address.",   "1234"
   "dob",                             "string",   "Date of birth of the policy holder.",   "1989-02-04"
   "housemates",                      "array",    "Empty array.", "[]"
   "items",                           "array",    "Array of items insured",  "[]"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'Content-Type: application/json' \
        -d '{
          "data": {
              "city": "Basel",
              "dob": "1990-01-01",
              "house_number": "12",
              "housemates": [],
              "items": [
                  {
                      "name": "Natel",
                      "type": "phone"
                  },
                  {
                      "name": "Kamera",
                      "type": "camera"
                  },
                  {
                      "name": "Velo",
                      "type": "bike"
                  }
              ],
              "phone": "+41611111111",
              "postcode": "4053",
              "street": "Some street"
          },
          "email": "test@kasko.io",
          "first_name": "First name",
          "language": "de",
          "last_name": "Last name",
          "quote_token": "quote_token",
          "referrer_url": "",
          "key": "<PUBLIC KEY>"
    }'

Convert Policy To Paid Request
------------------------------
After creating unpaid policy it is required to convert it to paid. This can be done by making another request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``stripe token`` returned by stripe."
   "policy_id", "yes", "``string``", "The 33 character long policy ID returned by the create unpaid policy request."
   "method", "yes", "``string``", "Payment method ``creditcard``."
   "provider", "yes", "``string``", "Payment provider ``stripe``."


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/payments' \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<STRIPE PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>",
            "method": "creditcard",
            "provider": "stripe",
            "key": "<PUBLIC KEY>"
        }'
