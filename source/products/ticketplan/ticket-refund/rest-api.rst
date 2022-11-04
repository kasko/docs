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
For quote input fields ``*`` is a number from 0 to 49.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "protected_element.*.price",         "integer",      "If module is ``banded``",               "3000"
   "max_ticket_price.*.single_banded",  "integer",      "If module is ``single``",               "45000"
   "pem_exclusion",                     "bool",  "",                                             "true|false"
   "sports_inclusion",                  "bool", "",                                              "true|false"



Example Request banded
~~~~~~~~~~~~~~~

.. code:: bash

   curl -X POST \
        'https://api.kasko.io/quotes' \
        -H 'Accept: application/vnd.kasko.v3+json' \
        -H 'Content-Type: application/json' \
        -d '{
        "key": "pk_test_e3c5e76e8bee4e0fb241e92e539ac258",
        "integration_id": "in_10ed87330bfabac47938858feeb85",
        "subscription_plan_id": "pp_575c17bb5bb298bdef8e5ca49f0ca",
        "item_id": "ins_7ce99aa9ffe9d26b627cf7735f74",
        "integration_version_id": "inv_25a563c290aa1de79cf2a2bcd943",
        "product_version_id": "prv_a61d1f1e3fe59e7510315c28ccef",
        "data": {
            "pem_exclusion": 1,
            "sports_inclusion": 0,
            "protected_element": {
                "0": {
                    "price": 3000
                }
            },
        }
}'

.. _QuoteResponse:

Example response banded
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

Example Request single
~~~~~~~~~~~~~~~

.. code:: bash

   curl -X POST \
        'https://api.kasko.io/quotes' \
        -H 'Accept: application/vnd.kasko.v3+json' \
        -H 'Content-Type: application/json' \
        -d '{
        "key": "pk_test_e3c5e76e8bee4e0fb241e92e539ac258",
        "integration_id": "in_f6e5410024f0484461bcc550e58a5",
        "subscription_plan_id": "pp_64ea33bae89ec7c6a651c94142211",
        "item_id": "ins_d329f5cf788fe195841b95205866",
        "live_integration": "false",
        "live_product": "false",
        "data": {
            "pem_exclusion": 1,
            "sports_inclusion": 0,
            "max_ticket_price": {
                "1": {
                    "single_banded": 5000
                }
            },
            "policy_start_date": "2022-11-31"
        }
}'

.. _QuoteResponse:

Example response single
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
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 100, 20, 80

   "booking_date",                    "yes",                                                "string",  "Booking date string."
   "payment_date",                    "yes",                                                "string",  "Payment date string."
   "ticket_quantity",                 "yes",                                                "integer",  "Ticket quantity."
   "order_number",                    "yes",                                                "integer",  "Order number."
   "event_name",                      "yes",                                                "string",  "Event name."
   "event_start_date",                "yes",                                                "string",  "Event start date string."
   "event_end_date",                  "yes",                                                "string",  "Event end date string."
   "venue_name",                      "yes",                                                "string",  "Venue name."
   "venue_location",                  "yes",                                                "string", "Venue location"
   "venue_country",                   "yes",                                                "string", "Venue country."
   "ticket_distributor",              "yes",                                                "string",  "Ticket distributor."
   "customer_email",                  "yes",                                                "string",  "Customer email."
   "customer_first_name",             "yes",                                                "string",  "Customer first name."
   "customer_last_name",              "yes",                                                "string",  "Customer last name."
   "customer_house_number",           "no",                                                 "string",  "Customer house number."
   "customer_street",                 "no",                                                 "string",  "Customer street."
   "customer_city",                   "no",                                                 "string",  "Customer city."
   "customer_postcode",               "no",                                                 "string",  "Customer postcode."
   "protected_elements_value",        "yes",                                                "integer",  "Protected elements value."
   "unprotected_elements_value",      "yes",                                                "integer",  "Unprotected elements value."
   "insurance_quantity",              "no",                                                 "integer",  "Insurance quantity."

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
                "booking_date": "2022-02-02",
                "payment_date": "2022-02-02",
                "ticket_quantity": 3,
                "order_number": 4,
                "order_value": 100000,
                "order_currency": "str",
                "event_name": "Test Name",
                "event_start_date": "2022-02-02",
                "event_end_date": "2022-02-03",
                "venue_name": "Venue Name",
                "venue_location": "Venue Location",
                "venue_country": "UK",
                "ticket_distributor": "Test Distributor",
                "customer_email": "example@test.com",
                "customer_first_name": "First",
                "customer_last_name": "Last",
                "customer_house_number": "123",
                "customer_street": "Street",
                "customer_city": "City",
                "customer_postcode": "1234",
                "protected_elements_value": 1234,
                "unprotected_elements_value": 1234,
                "insurance_quantity": 1
            },
            "quote_token":"<QUOTE TOKEN>",
            "first_name": "Test",
            "last_name": "Person",
            "email": "test@kasko.io",
        }'

NOTE. You should use ``<QUOTE TOKEN>`` value from QuoteResponse_.

.. _OfferResponse:

Example response
~~~~~~~~~~~~~~~~

.. code:: bash

    {
        "id": "<POLICY ID>",
        "insurer_policy_id": "TEST-XXXXXXX",
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
   "data",         "no",    "json",      "Data object."

Data object parameters if included in the policy update request.

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 100, 20, 80

   "booking_date",                    "yes",                                                "string",  "Booking date string."
   "payment_date",                    "yes",                                                "string",  "Payment date string."
   "ticket_quantity",                 "yes",                                                "integer",  "Ticket quantity."
   "order_number",                    "yes",                                                "integer",  "Order number."
   "event_name",                      "yes",                                                "string",  "Event name."
   "event_date",                      "yes",                                                "string",  "Event date string."
   "venue_name",                      "yes",                                                "string",  "Venue name."
   "venue_location",                  "yes",                                                "string", "Venue location"
   "venue_country",                   "yes",                                                "string", "Venue country."
   "ticket_distributor",              "yes",                                                "string",  "Ticket distributor."
   "customer_email",                  "yes",                                                "string",  "Customer email."
   "customer_first_name",             "yes",                                                "string",  "Customer first name."
   "customer_last_name",              "yes",                                                "string",  "Customer last name."
   "customer_house_number",           "no",                                                 "string",  "Customer house number."
   "customer_street",                 "no",                                                 "string",  "Customer street."
   "customer_city",                   "no",                                                 "string",  "Customer city."
   "customer_postcode",               "no",                                                 "string",  "Customer postcode."
   "protected_elements_value",        "yes",                                                "integer",  "Protected elements value."
   "unprotected_elements_value",      "yes",                                                "integer",  "Unprotected elements value."
   "insurance_quantity",              "no",                                                 "integer",  "Insurance quantity."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

     curl --location --request PATCH https://api.kasko.io/policies/<POLICY ID> \
        --header 'Accept: application/vnd.kasko.v2+json' \
        --header 'Authorization: Bearer <YOUR SECRET API KEY>' \
        --header 'Content-Type: application/json' \
        --data-raw '{
            "data": {
                "booking_date": "2022-02-02",
                "payment_date": "2022-02-02",
                "ticket_quantity": 3,
                "order_number": 4,
                "order_value": 100000,
                "order_currency": "str",
                "event_name": "Test Name",
                "event_date": "2022-02-02",
                "venue_name": "Venue Name",
                "venue_location": "Venue Location",
                "venue_country": "UK",
                "ticket_distributor": "Test Distributor",
                "customer_email": "example@test.com",
                "customer_first_name": "First",
                "customer_last_name": "Last",
                "customer_house_number": "123",
                "customer_street": "Street",
                "customer_city": "City",
                "customer_postcode": "1234",
                "protected_elements_value": 1234,
                "unprotected_elements_value": 1234,
                "insurance_quantity": 1
            },
            "first_name": "Test",
            "last_name": "Person",
            "email": "test@kasko.io"
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
