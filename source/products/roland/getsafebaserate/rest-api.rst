========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in Quote_, Offer_, Show_, Update_, Cancel_ requests.
2. ``If-Match: <ETag>`` - This header should be sent along with Update_ and Cancel_ requests. Value  for this header is ``etag`` headers value, it comes along with Show_ response. Use flag ``-v`` in your Show_ request to see it.
3. ``If-Unmodified-Since: <Date>`` - This header should be sent along with Update_ and Cancel_ requests. Value for this header is ``Last-Modified`` headers value,  it comes along with Show_ response as (RFC7232) Date. Use flag ``-v`` in your Show_ request to see it.

Possible requests
=================

At least 3 requests required in following order to buy policy:

1. Quote_ requests - get an estimate of policy costs.
2. Offer_ requests - create an offer.
3. Payment_ requests - covert offer to policy.

When a policy is created, the following actions can be taken:

4. Update_ request - update the policy quote ( price sensitive ) or other ( price insensitive ) information.
5. Cancel_ request - cancel the policy. Note: when the policy is cancelled, no more modification requests can be made.
6. Reactivate_ request - reactivate cancelled policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "partner_coverage_type",  "string", "Coverage Type for Partner in:single,spouse,partner",                "single"
   "main_module",            "string", "Insurance module from the following two: PBVIM or PBV",             "PBVIM"
   "postcode",               "string", "Postcode of the policy holders address. Must be 5 characters long", "99869"
   "duration",               "string", "Duration, always P1Y",                                              "P1Y"
   "dob",                    "string", "Policy holders date of birth. Must use ISO Date format",            "yyyy-mm-dd"
   "start_date",             "string", "Policy start date. Must use ISO Date format",                       "yyyy-mm-dd"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl -X POST https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=TOUCHPOINT_ID \
       -d item_id=ITEM_ID \
       -d subscription_plan_id=SUBSCRIPTION_ID \
       -d data='{
                "partner_coverage_type": "partner",
                "main_module": "PBV",
                "postcode": "99869",
                "duration": "P1Y",
                "dob": "1993-02-12",
                "start_date": "2019-11-15"
        }'

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "token":"<QUOTE_TOKEN>",
        "gross_payment_amount":1442,
        "extra_data": {
            "gross_premium":1442,
            "premium_tax":230,
            "net_premium":1212,
            "tax_rate":0.19
        }
    }

.. _Offer:

Create an offer (unpaid policy)
-------------------------------

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 35, 20, 75, 20

   "phone",                           "string|optional",   "A valid phone number",   "+417304200"
   "salutation",                      "string",            "Customer title. Available values: mr, ms.",   "mr"
   "house_number",                    "string",            "House number of the policyholder's address.",   "12"
   "street",                          "string",            "Street name of the policyholder's address.",   "Main street"
   "state",                           "string",            "State of the policyholder's address.",   "State"
   "previous_insurance_insurer",      "string|optional",   "Previous insurer name.",   "Insurer name"
   "previous_insurance_claims_count", "integer|optional",  "Previous insurance claim count.",   "2"
   "previous_insurance_cancellation", "integer|optional",  "Previous cancellation reason.",   "2"
   "previous_insurance_start_date",   "string|optional",   "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "previous_insurance_end_date",     "string|optional",   "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "partner_coverage_type",           "string",            "Partner coverage type. in:single,spouse,partner",     "single"
   "coinsured_first_name",            "string|optional",   "Co-insured first name. Required if ``partner_coverage_type`` is ``partner``.",   "FirstName"
   "coinsured_last_name",             "string|optional",   "Co-insured fast name. Required if ``partner_coverage_type`` is ``partner``.",   "LastName"

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
                "phone": "+44 117 496 0123",
                "salutation": "mr",
                "house_number": "1A",
                "street": "Test Street",
                "state": "Test State",
                "partner_coverage_type": "partner",
                "coinsured_first_name": "firstName",
                "coinsured_last_name": "lastName"
          },
          "quote_token":"<QUOTE TOKEN>",
          "first_name": "Test",
          "last_name": "Person",
          "email": "test@kasko.io",
          "language": "de"
      }'

NOTE. You should use ``<QUOTE TOKEN>`` value from QuoteResponse_.

.. _OfferResponse:

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "id": "<POLICY ID>",
        "insurer_policy_id": "TEST-ROLANDGS-XXXXXXX",
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
        -u YOUR SECRET API KEY: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "PAYMENT TOKEN",
            "policy_id": "POLICY ID",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.

.. _Show:

Show policy of id
-----------------

Example Request
~~~~~~~~~~~~~~~
.. code-block:: bash

    curl -X GET https://api.kasko.io/policies/POLICY_ID \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json'

Note you should use ``<POLICY ID>`` from OfferResponse_ in order to retrieve policy data.

.. _ShowResponse:

Example response
~~~~~~~~~~~~~~~~

The response will contain policy data in the response body. Also, response headers ``Last-Modified`` and ``Etag`` will be exposed. To see these headers, add ``-v`` flag to your request.

.. _Update:

Update policy
-------------

JSON data sent in policy update request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "first_name",  "no", "string", "Policy holder name."
   "last_name",   "no", "string", "Policy holder surname"
   "email",       "no", "string", "Policy holder email address."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

 curl --location --request PATCH https://api.kasko.io/policies/<POLICY ID> \
--header 'Accept: application/vnd.kasko.v2+json' \
--header 'Authorization: Bearer <YOUR SECRET API KEY>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "first_name": "John",
    "email": "test+2@kasko.io"
}'

NOTE. You should use ``<POLICY ID>``, ``<Etag>`` and ``<Last-Modified>`` from ShowResponse_.

.. _Cancel:

Cancel policy request
---------------------

JSON data sent in policy cancellation request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "status",              "yes", "string",   "Policy status ``cancelled``."
   "cancellation_reason", "yes", "string",   "Reason why policy is being cancelled."
   "termination_date",    "no",  "string",   "Date on which policy was terminated in ISO 8601 format (YYYY-mm-dd)."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/policies/<POLICY ID> \
        -X PUT \
        -u YOUR_SECRET_API_KEY: \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'If-Match: ETAG_HEADER' \
        -H 'If-Unmodified-Since: LAST_MODIFIED_HEADER' \
        -H 'Content-Type: application/json' \
        -d '{
            "status": "cancelled",
            "cancellation_reason": "Specify your reason here",
            "termination_date": "2018-12-18"
        }'

NOTE. You should use ``<POLICY ID>``, ``<Etag>`` and ``<Last-Modified>`` from ShowResponse_.


.. _Reactivate:

Reactivate policy request
---------------------

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/policies/<POLICY ID>/reactivate \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Accept: application/vnd.kasko.v2+json'
