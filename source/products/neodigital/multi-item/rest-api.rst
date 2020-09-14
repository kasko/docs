========
REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in Quote_ and Offer_ requests.

Possible requests
=================

At least 3 requests required in following order to buy policy:

1. Quote_ requests - get an estimate of policy costs.
2. Offer_ requests - create an offer.
3. Payment_ requests - covert offer to policy.

Quote Data For Low Turnover
-------------------------------------------

.. _Quote:

This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "item_age",            "string", "Age of the item. In: max_2_weeks, max_3_months, max_6_months, older", "older"
   "item_value",          "string", "Item value. In: max_250, max_500, max_750, max_1000, max_1500, max_2000", "max_2000"
   "original_owner",      "bool",   "Is the policy buyer the original owner.", "true"
   "duration",            "string", "Policy duration. In: P1Y, P2Y", "P1Y"
   "deductible_selected", "bool",   "Deductible selected.", "false"
   "theft_module",        "bool",   "Theft Module", "true"
   "warranty_module",     "bool",   "Warranty Module", "true"
   "item_discount",       "string", "A dataset item id corresponding to item discount value. Id starting with '``dai_'``", "``dai_some_id_32_chars_long_______``"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X post  https://api.kasko.io/quotes \
       -u <sk_test_SECRET_KEY>: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=<TOUCHPOINT ID> \
       -d item_id=<ITEM ID> \
       -d subscription_plan_id=<SUBSCRIPTION PLAN ID> \
       -d data='{
            "item_age": "max_2_weeks",
            "item_value": "max_2000",
            "original_owner": true,
            "duration": "P1Y",
            "deductible_selected": false,
            "theft_module": false,
            "warranty_module": true,
            "item_discount": "dai_0AeGjJLerkYUyH3kxqkqsHXF01Oa"
        }'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

    {
        "token": "<QUOTE_TOKEN>",
        "gross_payment_amount": 22098,
        "extra_data": {
            "gross_premium": 22098,
            "premium_tax": 3528,
            "net_premium": 18570,
            "tax_rate": 0.19,
            "total_gross_premium": 22098,
            "total_tax_premium": 3528,
            "total_net_premium": 18570,
            "deductible_share": 20000,
            "warranty_module_gross_premium": 1377,
            "theft_module_gross_premium": 3985,
            "policy_start_date": "2020-09-02",
            "policy_end_date": "2021-09-02"
        }
    }

Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "discount_code",               "string", "Discount code. Regexp - /^$/. This is an optional value", "Not used at the moment"
   "item_identifier",             "string", "A valid IMEI number.", "998653879347201"
   "individual_description",      "bool",   "Optional individual description value", "true"
   "item_description",            "string", "Required if individual_description = false. This is a dataset item id", "dai_some_id_32_chars_long_______"
   "item_description_individual", "string", "Required if individual_description = true", "Apple 32"
   "salutation",                  "string", "Policyholders salutation.", "ms"
   "dob",                         "string", "Date of birth.", "1993-02-12"
   "country",                     "string", "This is a dataset item id", "dai_some_id_32_chars_long_______"
   "postcode",                    "string", "Policyholders Postcode", "12345"
   "city",                        "string", "Policyholders City", "Test City"
   "street",                      "string", "Policyholders street", "Test st"
   "house_number",                "string", "Policyholders street", "52"
   "newsletter_optin",            "bool",   "Newsletter optin", "true"
   "iban",                        "string", "Iban number", "DE89370400440532013000"
   "bic",                         "string", "Optional Bic number", "AARBDE5W200"
   "different_account_holder",    "bool",   "Is there a different account holder", "false"
   "show_account_holder_info",    "bool",   "Required if different_account_holder = true. Should we show account holder info", "false"
   "account_holder_salutation",   "string", "Required if different_account_holder = true. Account holder salutation", "mr"
   "account_holder_first_name",   "string", "Required if different_account_holder = true. Account holders first name", "testName"
   "account_holder_last_name",    "string", "Required if different_account_holder = true. Account holders last name", "lastName"
   "account_holder_postcode",     "string", "Required if different_account_holder = true. Account holders postcode", "12312"
   "account_holder_city",         "string", "Required if different_account_holder = true. Account holders city", "Test City"
   "account_holder_street",       "string", "Required if different_account_holder = true. Account holders street", "Test St"
   "account_holder_house_number", "string", "Required if different_account_holder = true. Account holders house number", "100"

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
               "item_identifier": "998653879347201",
               "item_description": "dai_0AeGjJLerkYUyH3kxqkqsHXF01Oa",
               "salutation": "ms",
               "dob": "1993-02-12",
               "country": "dai_1Jt16WAlv61bq9Fh56T6x9NiIN6I",
               "postcode": "12345",
               "city": "Test City",
               "street": "Test St",
               "house_number": "42",
               "newsletter_optin": false,
               "iban": "DE89370400440532013000",
               "different_account_holder": true,
               "show_account_holder_info": true,
               "account_holder_salutation": "ms",
               "account_holder_first_name": "First Name",
               "account_holder_last_name": "Last Name",
               "account_holder_postcode": "12312",
               "account_holder_city": "Test City",
               "account_holder_street": "Test St",
               "account_holder_house_number": "100"
          },
          "quote_token":"<TOKEN>",
          "first_name": "FirstName",
          "last_name": "LastName",
          "email": "example@kasko.io",
          "language": "de"
      }'

Example response
~~~~~~~~~~~~~~~~
.. _OfferResponse:

.. code:: bash

    {
        "id": "<POLICY_ID>",
        "insurer_policy_id": "<INSURER_POLICY_ID>",
        "payment_token": "<TOKEN>",
        "_links": {
            "_self": {
                "href": "https:\/\/api.eu1.kaskocloud.com\/policies\/<POLICY_ID>"
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
