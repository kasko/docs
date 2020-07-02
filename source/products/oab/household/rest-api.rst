========
Rest api
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in `Quote request`_, `Offer`_ requests.

Possible requests
=================

To purchase a policy at least 3 requests are required in the following order:

1. `Quote`_  - get the policy price.
2. `Offer`_ - create an offer.
3. `Payment`_ convert offer to a paid policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Required", "Example Value"
   :widths: 20, 20, 80, 20

   "inhabitation",            "string", "YES", "in:inhabitated,uninhabitated"
   "building_material",       "string", "YES", "in:massive,wood"
   "roof_material",           "string", "YES", "in:regular,weak"
   "interior",                "string", "YES", "in:simple,average,premium"
   "area",                    "integer", "YES", "max:300|min:0"
   "insured_sum",             "integer","YES", "max:520000|min:0"
   "schutzbrief_module",      "string", "YES", "in:none,basic,plus"
   "bicycle_module",          "string", "YES", "in:theft,kasko"
   "bicycle_additional_true", "bool", "YES if:bicycle_module = theft", "true"
   "bicycle_additional_sum",  "integer", "YES if:bicycle_additional_true = true", "max:500000"
   "bicycle_type",            "string", "YES if:bicycle_module = kasko", "in:ebike,regular_bike"
   "bicycle_built_year",      "integer", "YES if:bicyle_module = kasko", "2020"
   "bicycle_value",           "string", "YES if:bicycle_module = kasko", "max:750000"
   "fairplay_module",         "bool", "YES", "true"
   "glass_module",            "bool", "YES", "false"
   "element_module",          "bool", "YES", "true"
   "previous_damages_element", "bool", "YES", "true"
   "postcode",                "string", "YES", "64277"
   "duration",                "string", "YES", "P1Y"
   "previous_damages_true",   "bool", "YES", "true"
   "payment_frequency",       "string", "YES", "frequency"
   "policy_start_date",       "string","YES", "Policy start date `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD). 2020-07-01"
   "previous_insurance",      "bool", "YES", "true"
   "previous_end_date",       "string", "YES: if:previous_insurance = true", "`ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD). 2020-06-15"
   "cancellation",            "string", "YES: if:previous_insurance = true", "in:no_cancellation,user_cancellation,insurer_cancellation"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
        -u sk_test_SECRET_KEY: \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -d touchpoint_id=tp_b8b12a2df7300d7c778f30fda2168 \
        -d item_id=item_211e7339a684eda0d69f5c7a320 \
        -d subscription_plan_id=sp_5c424b178c30d891f0286f2be2fae \
        -d data='{"inhabitation":"inhabitated","building_material":"massive","roof_material":"regular","interior":"simple","area":8,"insured_sum":520000,"schutzbrief_module":"none","bicycle_module":"theft","bicycle_additional_true":false,"bicycle_additional_sum":500000,"fairplay_module":false,"glass_module":false,"element_module":false,"previous_damages_element":false,"postcode":"01067","duration":"P1Y","previous_damages_true":false,"payment_frequency":"yearly","policy_start_date":"2020-07-01","previous_insurance":false,"cancellation":"no_cancellation"}'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

    {
        "token": "<QUOTE TOKEN>",
        "gross_payment_amount": 27084,
        "extra_data": {
            "gross_premium": 3507,
            "premium_tax": 488,
            "net_premium": 3019,
            "tax_rate": 0.1615,
            "bicycle_coverage_included": 5200,
            "bicycle_coverage_add_cost": 7471,
            "min_insured_sum": 520000,
            "frequency_gross_premium": 3507,
            "fairplay_module": 109,
            "glass_module": 3280,
            "element_module": 121,
            "schutzbrief_module_basic": 4000,
            "schutzbrief_module_plus": 6400,
            "bicycle_kasko_add_cost": 12852
        }
    }


Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

    "first_name",          "string", "First name.", "Arturs"
    "last_name",           "string", "Last name.", "Jerjomins"
    "optin1",              "bool", "Electronic data transfer option.", "true"
    "optin2",              "bool", "Newsletter option.", "false"
    "salutation",          "string", "Salutation.", "mr"
    "title",               "string", "Title.", "the title"
    "phone",               "string", "Phone number.", "+44222222222"
    "dob",                 "string", "Date of birth `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD).", "1990-08-01"
    "previous_insurer",    "name", "Required if:previous_insurance = true. Name of previous insurance company", "oab"
    "insurance_number",    "string", "Insurance number.", "111_333"
    "bicycle_make",        "string", "Required if:bicycle_module = kasko", "customer"
    "bicycle_model",       "string", "Required if:bicycle_module = kasko", "test"
    "bicycle_module",      "string", "In:theft,kasko", "kasko"
    "member_id",           "string", "Member id.", "1234az"
    "street",              "string", "Street", "aj street"
    "city",                "string", "City.", "riga"
    "house_number",        "string", "House number", "111z"
    "previous_insurance",  "bool", "Previous insurance of user available?", "true"
    "account_holder_name", "string", "Account holder name.", "AJ Jerjomins"
    "iban",                "bool",   "Iban number", "XXXXXXXXXXXXXXXXXXX"
    "bank_name",           "string", "Bank name.", "Grizl bank"

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
                "optin1": true,
                "optin2": true,
                "house_number": "12",
                "phone": "+41840000000",
                "postcode": "1010",
                "city": "Riga",
                "salutation": "mr",
                "street": "test",
                "title": "ohne",
                "dob": "1990-08-01",
                "previous_insurer": "AJ",
                "insurance_number": "112z",
                "bicycle_module": "theft",
                "member_id": "113z",
                "previous_insurance": true
            },
            "metadata": {
                "account_holder_name": "Test Test",
                "iban": "DE89370400440532013000",
                "bank_name": "aj bank"
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
            "token": "<PAYMENT_TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from `OfferResponse`_. After payment is made, policy creation is asynchronous.
