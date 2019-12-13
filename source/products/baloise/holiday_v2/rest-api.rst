========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in `Quote request`_, `Offer`_, `Show`_ requests.

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

   "camper",                    "bool",   "NO", "TRUE"
   "liability_cover",           "bool",   "NO", "TRUE"
   "cancellation_costs_cover",  "bool",   "NO", "TRUE"
   "trip_start_date",           "string", "YES",  "2020-08-01"
   "trip_end_date",             "string", "YES",  "2020-08-15"
   "luggage_insurance",         "bool",   "YES", "TRUE"
   "luggage_value",             "int",    "YES if:luggage_insurance = true", "2000"
   "reduced_excess",            "bool",   "YES", "TRUE"
   "cancellation_costs_value",  "int",    "YES if:cancellation_costs_cover = true", "20000"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=tp_a7461d7cf4b247fc6b472e2abc41d \
       -d item_id=item_d1afab6f436e44c399677edb680 \
       -d subscription_plan_id=sp_a8fd68e115fd5d670f13c9ebcca0f \
       -d data='{"camper":true,"liability_cover":true,"cancellation_costs_cover":true,"trip_start_date":"2020-08-01","trip_end_date":"2020-08-15","luggage_insurance":true,"luggage_value":5000,"reduced_excess":true,"cancellation_costs_value":100000}'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

    {
        "token": "<QUOTE TOKEN>",
        "gross_payment_amount": 42000,
        "extra_data": {
            "luggage_deductible": 5000,
            "price_before_discount": 42000,
            "gross_premium": 42000,
            "premium_tax": 6706,
            "net_premium": 35294,
            "net_commission_total": 0,
            "net_net_premium": 35294,
            "price_before_discount": 42000
        }
    }


Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

    "phone",        "string", "Phone number.", "+44222222222"
    "salutation",   "string", "Salutation.", "mr"
    "dob",          "string", "Date of birth `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD).", "1990-08-01"
    "street",       "string", "Street name.", "First street"
    "city",         "string", "City.", "London"
    "house_number", "string", "House number.", "1234"
    "postcode",     "string", "Postcode of the first residence owner's address.", "1234"

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
                "phone":"+44222222222",
                "salutation":"mr",
                "dob":"1990-08-01",
                "street":"First street",
                "city":"London",
                "house_number":"1234",
                "postcode":"1234"
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
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from `OfferResponse`_. After payment is made, policy creation is asynchronous.
