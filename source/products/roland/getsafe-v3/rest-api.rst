========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v3+json`` - V3 header. The API requests must include the v3 header in Quote_.
2. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in Offer_, Show_, Update_, Cancel_ requests.
3. ``If-Match: <ETag>`` - header that should be sent along with Update_ and Cancel_ requests. As ``<ETag>`` value should be used header ``Etag`` from Show_ response.
4. ``If-Unmodified-Since: <Date>`` - header that should be sent along with Update_ and Cancel_ requests. As ``<Date>`` value (RFC7232) should be used header ``Last-Modified`` from Show_ response.

Possible requests
=================

At least 3 requests required in following order to buy policy:

1. Quote_ requests - get an estimate of policy costs.
2. Offer_ requests - create an offer.
3. Payment_ requests - convert offer to policy.

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

   "duration",              "string", "Insurance duration. Available values:", "P1Y"
   "start_date",            "iso_date", "Start date of policy  ISO date", "yyyy-mm-dd"
   "main_module",           "string",  "Which insurance module", "PBV|PBVIM"
   "criminal_law",          "bool",  "Extra module for criminal law", "true|false"
   "module",                "string", "Which Tarif", "base"
   "familystatus",          "string", "Family status", "single"
   "employmentstatus",      "string", "Employment status", "employee"
   "dob",                   "string", "Date of birth", "1984-01-02"
   "postcode",              "string", "Postcode", "69115"
   "credit_score_external", "integer", "Credit rating", "999"



Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl -X POST \
        'https://api.kasko.io/quotes' \
        -H 'Accept: application/vnd.kasko.v3+json' \
        -H 'Content-Type: application/json' \
        -d '{
        "item_id": "item_2aa907bf7dbafb8a596468ddb43",
        "integration_id": "tp_970ae312b91606f28ba1f9873a0a9",
        "subscription_plan_id": "sp_65b68199141576fb1d65ba326c93c",
        "key": "sk_test_10p4DsBJyZloewSxpMSA9W0ObQejKu9l",
        "data":
            {"duration":"P1Y","start_date":"2021-11-25","main_module":"PBV","criminal_law":false,"module":"base","familystatus":"single","employmentstatus":"employee","dob":"1984-01-02","postcode":"69115","credit_score_external":999}
        }'

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

   "phone",                           "string|optional",   "Free text string up to 255 characters.",        "+417304200"
   "salutation",                      "string",            "Customer title. Available values: mr, ms.",     "mr"
   "dob",                             "string",            "Date of birth of the policyholder.",            "1989-02-04"
   "house_number",                    "string",            "House number of the policyholder's address.",   "12"
   "street",                          "string",            "Street name of the policyholder's address.",    "Main street"
   "state",                           "string",            "State of the policyholder's address.",          "State"
   "postcode",                        "string",            "Postcode of the policyholder's address.",       "1234"
   "partner_coverage",                "bool",              "Partner coverage.",                             "true"
   "familystatus",                    "string",            "Family status.",                                "single"

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
                "phone":"+4915209121603",
                "salutation": "mr",
                "dob": "1991-10-31",
                "house_number": "1A",
                "street": "Test Street",
                "state": "Test State",
                "postcode": "1001",
                "partner_coverage": false,
                "familystatus": "single"
          },
          "quote_token":"<QUOTE TOKEN>",
          "first_name": "Test",
          "last_name": "Person",
          "email": "test@kasko.io",
          "language": "de",
          "metadata": {}
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

   "first_name", "no", "string", "Policy holder name."
   "last_name", "no", "string", "Policy holder surname"
   "email", "no", "string", "Policy holder email address."

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
