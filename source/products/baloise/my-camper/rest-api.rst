========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in Quote_, Offer_, Show_ requests.

Possible requests
=================

To purchase a policy at least 3 requests are required in the following order:

1. Quote_ requests - get the policy price.
2. Offer_ requests - create an offer.
3. Payment_ requests - covert offer to a paid policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "cart_value",           "int",    "Cart value, max 5000000.", "50000"
   "duration",             "string", "P5D (Per 5 Days).", "P5D"
   "private_liability",    "bool",   "Private liability.", "TRUE"
   "comprehensive_damage", "bool",   "Comprehensive damage.", "TRUE"
   "deductible_insurance", "bool",   "Deductible insurance.", "TRUE"
   "car_interior",         "bool",   "Car interior.", "TRUE"
   "luggage",              "bool",   "Luggage.", "TRUE"
   "luggage_insured_sum",  "int",    "Sum of luggage 500000 or 1000000. (Rquired if luggage = true", "1000000"
   "annulation_cost",      "bool",   "Annulation cost.", "TRUE"
   "breakdown",            "bool",   "Breakdown.", "TRUE"
   "policy_start_date",    "string", "Policy start date `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD).", "2019-08-01"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=tp_7db25312d7e45d3d45618eba5bb74 \
       -d item_id=item_33e03cd6862c15059e83642a045 \
       -d subscription_plan_id=sp_965a0917e8c77eaff35e7699fea8b \
       -d data='{"cart_value":50000,"duration":"P5D","private_liability":true,"comprehensive_damage":true,"deductible_insurance":true,"car_interior":true,"luggage":true,"luggage_insured_sum":500000,"annulation_cost":true,"breakdown":true,"policy_start_date":"2019-08-01"}'

.. _QuoteResponse:

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "token": "<QUOTE TOKEN>",
        "gross_payment_amount": 24200,
        "extra_data": {
            "gross_premium": 24200,
            "premium_tax": 1138,
            "net_premium": 23062,
            "tax_rate": 0.05,
            "private_liability_gross_premium": 4500,
            "comprehensive_damage_gross_premium": 4500,
            "deductible_insurance_gross_premium": 2500,
            "car_interior_gross_premium": 3500,
            "luggage_gross_premium": 2700,
            "annulation_cost_gross_premium": 3000,
            "breakdown_gross_premium": 3500
        }
    }

.. _Offer:

Create an offer (unpaid policy)
-------------------------------

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

    "phone",        "string", "Phone number.", "+11111111"
    "salutation",   "string", "Salutation.", "mr"
    "dob",          "string", "Date of birth `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD).", "1990-08-01"
    "street",       "string", "Street name.", "First street"
    "city",         "string", "City.", "London"
    "house_number", "string", "House number.", "1234"
    "postcode",     "string", "Postcode of the first residence owner's address.", "1234"
    "booking_number", "string", "Booking number.", "1234"
    "booking_value", "string", "Booking value.", "1234"

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
                "phone":"+11111111",
                "salutation":"mr",
                "dob":"1990-08-01",
                "street":"First street",
                "city":"London",
                "house_number":"1234",
                "postcode":"1234",
                "booking_number": "1234",
                "booking_value": "1234"
          },
          "quote_token":"<QUOTE TOKEN>",
          "first_name": "Test",
          "last_name": "Person",
          "email": "test@kasko.io",
          "language": "en"
      }'

NOTE. You should use ``<QUOTE TOKEN>`` value from QuoteResponse_.

.. _OfferResponse:

Example response
~~~~~~~~~~~~~~~~

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

.. _Payment:

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
        -u sk_test_SECRET_KEY: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.

.. _Show:

Show policy by id
-----------------

Example Request
~~~~~~~~~~~~~~~
.. code-block:: bash

    curl -X GET https://api.kasko.io/policies/<POLICY ID> \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -u sk_test_SECRET_KEY: \
        -H 'Content-Type: application/json'

Note you should use ``<POLICY ID>`` from OfferResponse_ in order to retrieve policy data.
