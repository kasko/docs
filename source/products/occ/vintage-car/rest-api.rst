========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in `Quote request`_, `Offer`_ requests.

Possible requests
=================

To purchase a policy at least 3 requests are required in the following order:

1. `Quote request`_  - get the policy price.
2. `Offer`_ - create an offer.
3. `Payment`_ requests - covert offer to a paid policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Required", "Example Value"
   :widths: 20, 20, 80, 20

   "vehicle_type",         "string", "YES", "car"
   "built_year",           "string", "YES", "1890"
   "max_horse_power",      "bool",   "YES", "true"
   "car_type",             "string", "YES",  "dat_FZNnm3OFqmyYDEjjBr6Hf8eoXKm6"
   "insured_sum",          "integer","YES",  "150000"
   "recovery_cost",        "bool",   "YES", "true"
   "coverage_type",        "string", "YES", "half"
   "deductible_a1",        "string", "YES if:coverage_type = half", "15000"
   "deductible_b2",        "string", "YES if:coverage_type = full", "50000"
   "deductible_b1",        "string", "YES if:coverage_type = full", "30000"
   "daily_usage",          "bool",   "YES", "true"
   "daily_usage_car",      "bool",   "YES", "true"
   "garage",               "string", "YES", "other"
   "usage",                "string", "YES", "3000km"
   "first_registration",   "string", "YES", "1995-01-01"
   "driving_license_year", "string", "YES", "1996"
   "dob",                  "string", "YES", "1985-01-01"
   "young_driver",         "bool",   "YES", "true"
   "damages",              "bool",   "YES", "true"
   "lost_license",         "bool",   "YES", "true"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=tp_e7c93465988011b6cd655c287098a \
       -d item_id=item_6302c29ba4654b5b147c7fb123d \
       -d subscription_plan_id=sp_c1a356222a3e987c683ba6094a08f \
       -d data='{"built_year":"1995","car_type":"dai_bZ61xAv4zRErXeornq5Y41IS7LZ3","coverage_type":"full","daily_usage":false,"daily_usage_car":true,"damages":false,"deductible_a1":"15000","deductible_b1":"50000","deductible_b2":"15000","dob":"1985-01-01","driving_license_year":"2000","first_registration":"1995-01-01","garage":"single","insured_sum":"270000","lost_license":false,"max_horse_power":false,"policy_start_date":"2020-02-06","recovery_cost":false,"usage":"3000km","vehicle_type":"car","young_driver":false}'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

    {
        "token": "<QUOTE TOKEN>",
        "gross_payment_amount": 27084,
        "extra_data": {
            "gross_premium": 27084,
            "maximum_value": 500000,
            "minimum_value": 100000,
            "net_premium": 24400,
            "premium_tax": 2684,
            "suggested_sum": 270000,
            "tax_rate": 0.11
        }
    }


Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

    "phone",                       "string", "Phone number.", "+44222222222"
    "salutation",                  "string", "Salutation.", "mr"
    "dob",                         "string", "Date of birth `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD).", "1990-08-01"
    "street",                      "string", "Street name.", "first street"
    "city",                        "string", "City.", "dai_JfRu8a3ARWE7SVCBD1dGPOYZIyjJ"
    "house_number",                "string", "House number.", "1234"
    "postcode",                    "string", "Postcode of the first residence owner's address.", "1234"
    "newsletter_optin",            "bool",   "Agree of newsletter.", "true"
    "title",                       "string", "Title.", "dr_jur"
    "user",                        "string", "User", "customer"
    "car_id",                      "string", "Required if:new_client = false.", "test"
    "miles_value",                 "string", "Miles value.", "1234"
    "miles",                       "string", "Miles or km", "km"
    "license_plate_number",        "string", "License plate number.", "1234"
    "license_plate_number_prefix", "string", "License plate number prefix", "dat_VcWIvURQSDyDI3aKayGP4nnpLJew"
    "license_plate_type",          "string", "License plate type.", "shared"
    "new_client",                  "bool",   "New client?", "true"
    "horse_power",                 "string", "Horse power.", "1234"
    "maker_model",                 "string", "Maker model.", "1234"
    "maker",                       "string", "Maker.", "1234"

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
                "car_id": "1234",
                "city": "dai_JfRu8a3ARWE7SVCBD1dGPOYZIyjJ",
                "horse_power": "115",
                "house_number": "12",
                "license_plate_number": "ABCD",
                "license_plate_number_prefix": "dai_lCjGRcaZvAAds4WU17CqNIEXTcjp",
                "license_plate_type": "single",
                "maker": "Alfa Romeo",
                "maker_model": "155",
                "miles": "km",
                "miles_value": "0",
                "new_client": false,
                "newsletter_optin": true,
                "phone": "+41840000000",
                "postcode": "1010",
                "salutation": "mr",
                "street": "test",
                "title": "ohne",
                "user": "customer"
            },
            "quote_token":"<QUOTE TOKEN>",
            "first_name": "Test",
            "last_name": "Person",
            "email": "test@kasko.io",
            "language": "de"
    }'

NOTE. You should use ``<QUOTE TOKEN>`` value from `QuoteResponse`_.

Example response
~~~~~~~~~~~~~~~~
.. _OfferResponse:

.. code:: bash

    {
        "id": "<POLICY ID>",
        "insurer_policy_id": "<INSURER_POLICY_ID>",
        "payment_token": "<PAYMENT TOKEN>",
        "_links": {
            "_self": {
                "href": "https://api.kasko.io/policies/<POLICY ID>"
            }
        }
    }


Convert offer to policy (payment)
---------------------------------
.. _Payment:

To create a policy you should convert offer to policy. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by `OfferResponse`_."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by `OfferResponse`_."
   "method",    "yes", "``string``", "Payment method ``distributor``."
   "provider",  "yes", "``string``", "Payment provider ``distributor``."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u sk_test_SECRET_KEY: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "invoice",
            "provider": "ergo_invoice"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from `OfferResponse`_. After payment is made, policy creation is asynchronous.
