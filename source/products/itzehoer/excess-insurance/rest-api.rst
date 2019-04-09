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
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20

   "vehicle_type",            "string", Yes, "Car or Lorry", "car|lorry"
   "vehicle_power",           "integer", Yes,   "Vehicle Power in Kw (Max 999)", "1"
   "start_date",              "iso_date", Yes,  "Start date of policy  ISO date. Max 2 months in future", "yyyy-mm-ddTHH:MM:SS.000Z"
   "end_date",                "iso_date", Yes,  "End date of policy  ISO date.", "yyyy-mm-ddTHH:MM:SS.000Z"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u <SECRET KEY>: \
      -d '{
        "language": "de",
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID>",
        "subscription_plan_id": "<SUBSCRIPTION PLAN ID>",
        "data": {
            "vehicle_type": "car",
            "vehicle_power": 300,
            "start_date": "2019-04-10T12:00:00.000Z",
            "end_date": "2019-04-20T12:00:00.000Z"            
        }
    }'

Create Unpaid Policy Request
----------------------------
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20

   "company_name",                    "string", Yes,   "Company Name",   "Kasko LTD"
   "company_license",                 "string", No,   "Identification number of the company",   "12345"
   "house_number",                    "string", Yes,   "House number of the policyholder's address.",   "12"
   "street",                          "string", Yes,   "Street name of the policyholder's address.",   "Main street"
   "city",                            "string", Yes,   "City name.",  "Hamburg"
   "postcode",                        "string", Yes,   "Postcode of the policyholder's address.",   "10115"
   "license_plate",                   "string", Yes,   "Vehicle License Plate",   "B-AA-1234"
   "manufacture_number",              "string", No,   "Manufactorer code",   "1234"
   "serial_number",                   "string", Yes,   "Vehicle Serial Number",   "1234abnd1234abdn2"
   "model_code",                      "string", No,   "Identifier of the type/model of the car",   "1234"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'Content-Type: application/json' \
        -u <SECRET KEY>: \
        -d '{
          "data": {
              "company_name": "Kasko",
              "company_license": "12345",
              "city": "Hamburg",
              "house_number": "12",
              "postcode": "10115",
              "street": "Some street",
              "license_plate": "B-AA-1234",
              "manufacture_number": "12345",
              "serial_number": "1234abnd1234abdn2",
              "model_code": "1234"             
          },
          "email": "test@kasko.io",
          "first_name": "First name",
          "language": "de",
          "last_name": "Last name",
          "quote_token": "quote_token"
    }'

Convert offer to policy (payment)
---------------------------------

To create a policy you should convert offer to policy. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by OfferResponse_."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by OfferResponse_."
   "method",    "yes", "``string``", "Payment method ``distributor``."
   "provider",  "yes", "``string``", "Payment provider ``distributor``."


Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
