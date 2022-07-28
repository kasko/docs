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

When there is a policy created, the following actions can be taken:

4. Update_ request - update policy quote ( price sensitive ) or other information ( price insensitive ) information.
5. Cancel_ request - cancel the policy. Note when the policy is cancelled no more modification requests can be done.
6. Reactivate_ request - reactivate cancelled policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "duration",                "string",   "Insurance duration. Available values:", "P1Y"
   "start_date",              "iso_date",   "Start date of policy  ISO date", "yyyy-mm-dd"
   "main_module",             "string",  "Which insurance module", "PBV|PBVIM"
   "partner_coverage",        "bool",  "Coverage for partner", "true|false"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=TOUCHPOINT_ID \
       -d item_id=item_b22001a5cb67ddc81ce7db58647 \
       -d subscription_plan_id=sp_01a1cc12c98ccc5b2c7a2d1e9fa09 \
       -d data='{"duration":"P1Y","start_date":"2018-12-12","main_module":"PBV","partner_coverage":true}'

.. _QuoteResponse:

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "token": "<QUOTE TOKEN>",
        "gross_payment_amount": 2056,
        "extra_data": {
            "gross_premium": 2056,
            "premium_tax": 328,
            "net_premium": 1728,
            "tax_rate": 0.19
        }
    }

.. _Offer:

Create an offer (unpaid policy)
-------------------------------

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 35, 20, 75, 20

   "phone",                           "string|optional",   "Free text string up to 255 characters.",   "+417304200"
   "salutation",                      "string",   "Customer title. Available values: mr, ms.",   "mr"
   "dob",                             "string",   "Date of birth of the policyholder.",   "1989-02-04"
   "house_number",                    "string",   "House number of the policyholder's address.",   "12"
   "street",                          "string",   "Street name of the policyholder's address.",   "Main street"
   "state",                           "string",   "State of the policyholder's address.",   "State"
   "postcode",                        "string",   "Postcode of the policyholder's address.",   "1234"
   "previous_insurance_insurer",      "string|optional",   "Previous insurer name.",   "Insurer name"
   "previous_insurance_claims_count", "integer|optional",   "Previous insurance claim count.",   "2"
   "previous_insurance_cancellation", "integer|optional", "Previous cancellation reason.",   "2"
   "previous_insurance_start_date",   "string|optional", "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "previous_insurance_end_date",     "string|optional", "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "partner_coverage",                "bool", "Partner coverage.",   "true"
   "coinsured_first_name",            "string|optional",   "Co-insured First name. Required if ``partner_coverage`` is ``true``.",   "FirstName"
   "coinsured_last_name",             "string|optional",   "Co-insured Last name. Required if ``partner_coverage`` is ``true``.",   "LastName"

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
                "salutation": "mr",
                "dob": "1991-10-31",
                "house_number": "1A",
                "street": "Test Street",
                "state": "Test State",
                "postcode": "1001",
                "partner_coverage": false
          },
          "quote_token":"<QUOTE TOKEN>",
          "first_name": "Test",
          "last_name": "Person",
          "email": "test@kasko.io",
          "language": "de",
          "metadata": {
    	    "linked_policy": "DEMO-XXXX"
          }
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
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
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

    curl -X GET https://api.kasko.io/policies/<POLICY ID> \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json'

Note you should use ``<POLICY ID>`` from OfferResponse_ in order to retrieve policy data.

.. _ShowResponse:

Example response
~~~~~~~~~~~~~~~~

The response will contain policy data in the response body. Also, response headers ``Last-Modified`` and ``Etag`` will be exposed.

.. _Update:

Update policy
-------------

JSON data sent in policy update request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "first_name",   "no",    "string",    "Policy holder name."
   "last_name",    "no",    "string",    "Policy holder surname"
   "email",        "no",    "string",    "Policy holder email address."
   "quote_token",  "no",    "string",    "Quote token."
   "data",         "no",    "json",      "Data object."

Data object parameters if included in the policy update request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "phone",                           "no",                                     "string",  "Free text string up to 255 characters."
   "salutation",                      "yes",                                    "string",  "Customer title. Available values: mr, ms."
   "dob",                             "yes",                                    "string",  "Date of birth of the policyholder in ISO 8601 format."
   "house_number",                    "yes",                                    "string",  "House number of the policyholder's address."
   "street",                          "yes",                                    "string",  "Street name of the policyholder's address."
   "state",                           "yes",                                    "string",  "State of the policyholder's address."
   "postcode",                        "yes",                                    "string",  "Postcode of the policyholder's address."
   "previous_insurance_insurer",      "no",                                     "string",  "Previous insurer name."
   "previous_insurance_claims_count", "yes if ``previous_insurance_insurer``",  "integer", "Previous insurance claim count."
   "previous_insurance_cancellation", "no",                                     "integer", "Previous cancellation reason."
   "previous_insurance_start_date",   "no",                                     "string",  "Previous insurance start date in ISO 8601 format."
   "previous_insurance_end_date",     "yes if ``previous_insurance_insurer``",  "string",  "Previous insurance end date in ISO 8601 format."
   "partner_coverage",                "yes",                                    "bool",    "Partner coverage."
   "coinsured_first_name",            "yes if ``partner_coverage``",            "string",  "Co-insured First name."
   "coinsured_last_name",             "yes if ``partner_coverage``",            "string",  "Co-insured Last name."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

     curl --location --request PATCH https://api.kasko.io/policies/<POLICY ID> \
        --header 'Accept: application/vnd.kasko.v2+json' \
        --header 'Authorization: Bearer <YOUR SECRET API KEY>' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "first_name": "John",
            "last_name": "Doe",
            "email": "test+2@kasko.io",
            "quote_token":<YOUR QUOTE TOKEN>,
            "data": {
                "dob":"1984-01-01",
                "street":"Bergheimer Str.",
                "house_number":"147",
                "state":"Heidelberg",
                "postcode":"69115",
                "salutation":"mr",
                "partner_coverage":false
            }
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
   "termination_date",    "no", "string",    "Date on which policy was terminated in ISO 8601 format (YYYY-mm-dd)."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/policies/<POLICY ID> \
        -X PUT \
        -u <YOUR SECRET API KEY>: \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'If-Match: <Etag>' \
        -H 'If-Unmodified-Since: <Last-Modified>' \
        -H 'Content-Type: application/json' \
        -d '{
            "status": "cancelled",
            "cancellation_reason": "Specify your reason here",
            "termination_date": "2018-12-18"
        }'

NOTE. You should use ``<POLICY ID>``, ``<Etag>`` and ``<Last-Modified>`` from ShowResponse_.


.. _Reactivate:

Reactivate policy request
-------------------------

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/policies/<POLICY ID>/reactivate \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Accept: application/vnd.kasko.v2+json'
