========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in Quote_, Offer_, Show_, Update_, Cancel_ requests.
2. ``If-Match: <ETag>`` - This header should be sent along with Update_ and Cancel_ requests. Value  for this header is ``etag`` headers value, it comes along with Show_ response. Use flag ``-v`` in your Show_ request to see it.

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

    "module",                 "string",              "Insurance module from the following two: base and comfort",                "base"
    "noclaimscategory",       "integer|optional",    "No claims category",                                                        "3"
    "employmentstatus",       "string",              "in:publicservice,employee,selfemployed,student,pensioner,unemployed",       "selfemployed"
    "familystatus",           "string",              "in:single,singlewithchild,couple,familywithchild",                          "familywithchild"
    "credit_score_external",  "integer|optional",    "Credit score",                                                              "373"
    "main_module",            "string",              "in:PBV,PBVIM",                                                              "PBV"
    "postcode",               "string",              "Post code",                                                                 "69115"
    "duration",               "string",              "Duration. in:P1Y",                                                          "P1Y"
    "dob",                    "string",              "Policy holders date of birth. Must use ISO Date format",                    "yyyy-mm-dd"
    "start_date",             "string",              "Policy start date. Must use ISO Date format",                               "yyyy-mm-dd"
    "criminal_law",           "boolean",             "Criminal law",                                                              "false"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl --location --request POST 'https://api.kasko.io/quotes' \
    --header 'Accept: application/vnd.kasko.v2+json' \
    --header 'Authorization: Bearer sk_test_Mtj4bN00iF6Zv5nDY3mWvbHre6123456' \
    --header 'Content-Type: application/json' \
    --data-raw '{
       "data":{
            "duration":"P1Y",
            "start_date":"2022-10-01",
            "main_module":"PBV",
            "criminal_law":false,
            "module":"base",
            "familystatus":"singlewithchild",
            "employmentstatus":"employee",
            "dob":"2000-01-01",
            "postcode":"69115"
       },
       "touchpoint_id":"TOUCHPOINT_ID",
       "item_id":"ITEM_ID",
       "subscription_plan_id":"SUBSCRIPTION_ID"
    }'

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "token":"<QUOTE_TOKEN>",
        "gross_payment_amount": 1833,
        "extra_data": {
            "gross_premium": 1833,
            "premium_tax": 293,
            "net_premium": 1540,
            "tax_rate": 0.19,
            "yearly_gross_premium": 21990,
            "deductible": 30000,
            "yearly_net_premium": 18479
        }
    }

.. _Offer:

Create an offer (unpaid policy)
-------------------------------

This request stores policy holder information that is related to offer. Following information can be stored in offer:

Data sent in offer create request.

.. csv-table::
   :header: "Parameter", "Type", "Description", "Example Value"
   :widths: 20, 20, 20, 80

   "first_name",   "string",            "Policy holder name.",                                 "John"
   "last_name",    "string",            "Policy holder surname.",                              "Doe"
   "email",        "string",            "Policy holder email.",                                "john.doe@test.com"
   "language",     "string",            "Policy holder language.",                             "de"
   "quote_token",  "string",            "Quote token.",                                        "eyJpZCI6ImRyMCIsIml2IjoiUkpuaHJpeml2bG"
   "data",         "json",              "Data object.",                                        "data:{}"

Data object parameters in the offer create request.

.. csv-table::
   :header: "Parameter", "Type", "Description", "Example Value"
   :widths: 35, 20, 75, 20

   "phone",                           "string",                                                   "A valid phone number",                                "+417304200"
   "paymentperiod",                   "string|optional",                                          "in:yearly,monthly",                                   "yearly"
   "salutation",                      "string",                                                   "Customer title. Available values: mr, ms.",           "mr"
   "house_number",                    "string",                                                   "House number of the policyholder's address.",         "12"
   "street",                          "string",                                                   "Street name of the policyholder's address.",          "Main street"
   "state",                           "string",                                                   "State of the policyholder's address.",                "State"
   "previous_insurance_insurer",      "string|optional",                                          "Previous insurer name.",                              "Insurer name"
   "previous_insurance_claims_count", "integer|required_with:previous_insurance_insurer",         "Previous insurance claim count.",                     "2"
   "previous_insurance_cancellation", "string|optional",                                          "Previous cancellation reason.",                       "sample reason"
   "previous_insurance_start_date",   "string|optional",                                          "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "previous_insurance_end_date",     "string|required_with:previous_insurance_insurer",          "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "coinsured_first_name",            "string|optional",                                          "Co-insured first name.",                              "FirstName"
   "coinsured_last_name",             "string|optional",                                          "Co-insured fast name.",                               "LastName"
   "familystatus",                    "string|in:single,singlewithchild,couple,familywithchild",  "Family status",                                       "familywithchild"
   "is_married",                      "boolean|required_if:familystatus,couple,familywithchild",  "Is married",                                          "false"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST 'https://api.kasko.io/policies' \
    -H 'Accept: application/vnd.kasko.v2+json' \
    -H 'Authorization: Bearer sk_test_abcdbN00iF6Zv5nDY3mWvbHre6123456' \
    -H 'Content-Type: application/json' \
    -d '{
        "first_name": "John",
        "last_name": "Doe",
        "email": "test@test.com",
        "language": "de",
        "data":{
            "phone":"+41781234567",
            "paymentperiod":"yearly",
            "salutation":"mr",
            "house_number":"12",
            "street":"Main street",
            "state":"State",
            "previous_insurance_insurer":"Insurer name",
            "previous_insurance_claims_count":2,
            "previous_insurance_cancellation":"sample reason",
            "previous_insurance_start_date":"2021-05-10",
            "previous_insurance_end_date":"2021-05-10",
            "coinsured_first_name":"Alice",
            "coinsured_last_name":"Ward",
            "familystatus":"familywithchild",
            "is_married":false
        },
       "quote_token":"<QUOTE TOKEN>"
    }'

NOTE. You should use ``<QUOTE TOKEN>`` value from Quote_ response.

.. _OfferResponse:

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "id": "tpol_bc62ed24c87ff576c597082c0b64",
        "insurer_policy_id": "TEST-LEGALGS-B141WPPB",
        "payment_token": "eyJpZCI6ImRyMCIsIml2IjoiTnlcL",
        "_links": {
            "_self": {
                "href": "https://api.kasko.io/policies/policies/tpol_bc62ed24c87ff576c597082c0b64"
            },
            "assets": {
                "offer": {
                    "href": "https://api.kasko.io/policies/offers/tpol_bc62ed24c87ff576c597082c0b64/assets?token=asdfasdf"
                },
                "policy": {
                    "href": "https://api.kasko.io/policies/policies/tpol_bc62ed24c87ff576c597082c0b64/assets?token=asdfasdf"
                }
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
        -H 'Content-Type: application/json' \
        -H 'Authorization: Bearer sk_test_Mtj4bN00iF6Zv5nDY3mWvbHre6123456' \
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

    curl -v GET 'https://api.kasko.io/policies/tpol_bc62ed24c87ff576c597082c0b64' \
    --header 'Accept: application/vnd.kasko.v2+json' \
    --header 'Authorization: Bearer sk_test_Mtj4bN00iF6Zv5nDY3mWvbHre6123456'

Note you should use ``<POLICY ID>`` from OfferResponse_ in order to retrieve policy data.

.. _ShowResponse:

Example response
~~~~~~~~~~~~~~~~

The response will contain policy data in the response body. Also, response headers ``Etag`` will be exposed. To see these headers, add ``-v`` flag to your request.

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

   "phone",                           "string",                                                   "A valid phone number",                                "+417304200"
   "paymentperiod",                   "string|optional",                                          "in:yearly,monthly",                                   "yearly"
   "salutation",                      "string",                                                   "Customer title. Available values: mr, ms.",           "mr"
   "house_number",                    "string",                                                   "House number of the policyholder's address.",         "12"
   "street",                          "string",                                                   "Street name of the policyholder's address.",          "Main street"
   "state",                           "string",                                                   "State of the policyholder's address.",                "State"
   "previous_insurance_insurer",      "string|optional",                                          "Previous insurer name.",                              "Insurer name"
   "previous_insurance_claims_count", "integer|required_with:previous_insurance_insurer",         "Previous insurance claim count.",                     "2"
   "previous_insurance_cancellation", "string|optional",                                          "Previous cancellation reason.",                       "sample reason"
   "previous_insurance_start_date",   "string|optional",                                          "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "previous_insurance_end_date",     "string|required_with:previous_insurance_insurer",          "Previous insurance start date in ISO 8601 format.",   "YYYY-mm-dd"
   "coinsured_first_name",            "string|optional",                                          "Co-insured first name.",                              "FirstName"
   "coinsured_last_name",             "string|optional",                                          "Co-insured fast name.",                               "LastName"
   "familystatus",                    "string|in:single,singlewithchild,couple,familywithchild",  "Family status",                                       "familywithchild"
   "is_married",                      "boolean|required_if:familystatus,couple,familywithchild",  "Is married",                                          "false"

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

     curl --location --request PATCH 'https://api.kasko.io/policies/tpol_bc62ed24c87ff576c597082c0b64' \
     --header 'Accept: application/vnd.kasko.v2+json' \
     --header 'Authorization: Bearer sk_test_Mtj4bN00iF6Zv5nDY3mWvbHre6123456' \
     --header 'Content-Type: application/json' \
     --header 'If-Match: tpv_3447f95180b8d5e4894171fa85dbe' \
     --data-raw '{
         "first_name": "Janis",
         "email": "test+1@kasko.io",
         "data":{
             "phone":"+41781234567",
             "paymentperiod":"yearly",
             "salutation":"mr",
             "house_number":"12",
             "street":"Main street",
             "state":"State",
             "previous_insurance_insurer":"Insurer name",
             "previous_insurance_claims_count":2,
             "previous_insurance_cancellation":"sample reason",
             "previous_insurance_start_date":"2021-05-10",
             "previous_insurance_end_date":"2021-05-10",
             "coinsured_first_name":"Alice",
             "coinsured_last_name":"Ward",
             "familystatus":"familywithchild",
             "is_married":true
         }
     }'

NOTE. You should use ``<POLICY ID>``and ``<Etag>`` from ShowResponse_.

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
        -H 'Authorization: Bearer sk_test_Mtj4bN00iF6Zv5nDY3mWvbHre6123456' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'If-Match: ETAG_HEADER' \
        -H 'Content-Type: application/json' \
        -d '{
            "status": "cancelled",
            "cancellation_reason": "Specify your reason here",
            "termination_date": "2018-12-18"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<Etag>`` from ShowResponse_.


.. _Reactivate:

Reactivate policy request
-------------------------

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/policies/<POLICY ID>/reactivate \
        -X POST \
        -H 'Authorization: Bearer sk_test_Mtj4bN00iF6Zv5nDY3mWvbHre6123456' \
        -H 'Accept: application/vnd.kasko.v2+json'
