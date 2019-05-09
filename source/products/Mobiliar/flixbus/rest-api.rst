========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in Quote_, Offer_, Show_, Update_, Cancel_ requests.
2. ``If-Match: <ETag>`` - header that should be sent along with Update_ and Cancel_ requests. As ``<ETag>`` value should be used header ``Etag`` from Show_ response.
3. ``If-Unmodified-Since: <Date>`` - header that should be sent along with Update_ and Cancel_ requests. As ``<Date>`` value (RFC7232) should be used header ``Last-Modified`` from Show_ response.

Possible requests
=================

At least 3 requests required in following order to buy policy:

1. Quote_ requests - get an estimate of policy costs.
2. Offer_ requests - create an offer.
3. Payment_ requests - covert offer to policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "departure_location",                "string",   "Departure location", "zurich"
   "arrival_location",              "string",   "Arrival location", "grenoble"
   "departure_date",             "iso_date", "Start date of departure ISO date",  "yyyy-mm-dd"
   "departure_time",        "string",  "Start time of departure", "09:00"
   "delay",                  "int",  "Delay of the bus", "30"
   "payout_amount",        "int",  "Payout amount if bus is delayed", "35"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=tp_21716734231da536b06e0a624719f \
       -d item_id=item_e24329e6750469ee0bd5a92c65a \
       -d subscription_plan_id=sp_8f603535e76eb895124c9d2785c1b \
       -d data='{"departure_location":"zurich","arrival_location":"grenoble","departure_date":"2019-04-11","departure_time":"09:00","delay":0,"payout_amount":35}'

.. _QuoteResponse:

Sample response
~~~~~~~~~~~~~~~

.. code:: bash

    {
        "token": "<QUOTE TOKEN>",
        "gross_payment_amount": 307,
        "extra_data": {
            "gross_premium": 307,
            "premium_tax": 15,
            "net_premium": 292,
            "tax_rate": 0.05,
            "end_date": "2019-04-11 09:00",
            "start_date": "2019-04-11 09:01"
        }
    }

.. _Offer:

Create an offer (unpaid policy)
----------------------------

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 35, 20, 75, 20

   "phone",                           "string",   "Free text string up to 255 characters.",   "+417304200"

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
                "phone":"+11111111"
          },
          "quote_token":"<QUOTE TOKEN>",
          "first_name": "Test",
          "last_name": "Person",
          "email": "test@kasko.io",
          "language": "en"
      }'

NOTE. You should use ``<QUOTE TOKEN>`` value from QuoteResponse_.

.. _OfferResponse:

Sample response
~~~~~~~~~~~~~~~

.. code:: bash

    {
        "id": "<POLICY ID>",
        "insurer_policy_id": "TEST-MOB-34L3638J876",
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
   "method",    "yes", "``string``", "Payment method ``invoice``."
   "provider",  "yes", "``string``", "Payment provider ``invoice``."
   "metadata.iban",  "yes", "``string``", "``DE89370400440532013000``"
   "metadata.bic",  "yes", "``string``", "``BYLADEM1001``"


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
            "method": "invoice",
            "provider": "invoice",
            "metadata": {
                "iban": "DE89370400440532013000",
                "bic": "BYLADEM1001"
            }
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.

.. _Show:

Show policy of id
-----------------

Example Request
~~~~~~~~~~~~~~~
.. code-block:: bash

    curl -X GET https://api.kasko.io/policies/<POLICY ID> \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H <YOUR SECRET API KEY> \
        -H 'Content-Type: application/json'

Note you should use ``<POLICY ID>`` from OfferResponse_ in order to retrieve policy data.

.. _ShowResponse:
