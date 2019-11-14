=====================
Low Turnover REST API
=====================

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in `Quote request`_, `Offer`_, `Show`_ requests.

Quote Data for ZEP and Makler Low Turnover
-------------------------------------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "incorporation_date", "string", "Incorporation date in ISO date format and range from 1850-01-01 until tomorrow from the current day", "yyyy-mm-dd"
   "eoy_positive", "bool", "Were the earning at the end of year positive, true or false", "false"
   "sector", "string", "A dataset id corresponding to sector number always starting with `dai_", "dai_some_id_32_chars_long_______"
   "damage_claim", "bool", "Damage claim, true or false", "true"
   "single_damage", "bool", "Single damage required if damage claim = true, true or false", "true"
   "damage", "integer", "Damage, required if single damage = false. Min:0 Max:100000000000", "5000000"
   "turnover", "integer", "A numeric value from the following list: 99999900,299999900,499999900,1499999900,2499999900,4999999900", "2499999900"
   "insured_sum", "integer", "A numeric value from the following list: 10000000,25000000,50000000,100000000,200000000,250000000,300000000,400000000,500000000,600000000,700000000,750000000,800000000,900000000,1000000000", "1000000000"
   "policy_limit", "string", "Policy limit, single or double", "single"
   "policy_start_date", "string", "Start date of the policy in ISO date format. Starting from the current day +4 months in the future", "yyyy-mm-dd"
   "coverage_solid", "bool", "Coverage solid, true or false", "true"
   "subsidiary", "bool", "Subsidiary, true or false", "false"
   "public_company", "bool", "Public company, true or false", "false"
   "foreign_subsidiary", "bool", "Foreign subsidiary, true or false", "false"
   "previous_insurance", "bool", "Previous insurance, true or false", "false"
   "valid_contract", "bool", "Valid contract, required if previous insurance = true, true or false", "true"
   "cancelled_contract", "bool", "Cancelled contract, required if valid contract = false, true or false", "true"
   "missing_payment", "bool", "Missing payment, required if cancelled contract = false, true or false", "true"
   "equity_positive", "bool", "Is equity positive, true of false", "true"
   "damage_claims_expected_with_proffesional_activity", "bool", "Damage claims expected with proffesional activity, true of false", "true"

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
            "incorporation_date": "1989-02-03",
            "eoy_positive": true,
            "sector":"dai_some_id_32_chars_long_______",
            "damage_claim":false,
            "single_damage":false,
            "damage":"0",
            "turnover":99999900,
            "insured_sum":50000000,
            "policy_limit":"single",
            "policy_start_date":"2019-11-15",
            "coverage_solid":false,
            "subsidiary":false,
            "public_company":false,
            "foreign_subsidiary":false,
            "previous_insurance":false,
            "valid_contract":true,
            "cancelled_contract":true,
            "missing_payment":false,
            "equity_positive": true,
            "damage_claims_expected_with_proffesional_activity":false
        }'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

    {
        "token":"<QUOTE_TOKEN>",
        "gross_payment_amount":68820,
        "extra_data":{
            "gross_premium":68820,
            "premium_tax":6820,
            "net_premium":62000,
            "tax_rate":0.11,
            "insured_sum_higher":100000000
        }
    }

Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "company_name_with_legal_form", "string", "Company name", "Kasko LTD"
   "company_street", "string", "Street name of the companies address", "Green street"
   "company_house_number", "string", "House number of the companies address", "1"
   "company_postcode", "string", "Post code of the companies address, 4 numbers long", "1011"
   "company_city", "string", "Country of company", "London"
   "phone", "string", "Phone number", "+11111111"
   "salutation", "string", "Salutation, ms or mr", "mr"
   "agent_company", "string", "Agent company name", "AgentCompanyName"
   "agent_salutation", "string", "Agent salutation ms or mr", "ms"
   "agent_first_name", "string", "First name of the agent", "FirstName"
   "agent_last_name", "string", "Last name of the agent", "LastName"
   "agent_number", "string", "Agent number, 9 numbers long", "123456789"
   "agent_email", "string", "Agent email", "example@kasko.io"
   "agent_phone", "string", "Agent phone number", "+11111111"

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
                "company_name_with_legal_form": "Kasko LTD",
                "company_street": "Green Street",
                "company_house_number": "11",
                "company_postcode": "1011",
                "company_city": "London",
                "phone": "+11111111",
                "salutation": "ms",
                "agent_company": "AgentCompanyName",
                "agent_salutation": "mr",
                "agent_first_name": "FirstName",
                "agent_last_name": "LastName",
                "agent_number": "123456789",
                "agent_email": "example@kasko.io",
                "agent_phone": "+22222222"
          },
          "quote_token":"TOKEN",
          "first_name": "FirstName",
          "last_name": "LastName",
          "email": "example@kasko.io",
          "language": "en"
      }'

Example response
~~~~~~~~~~~~~~~~
.. _OfferResponse:

.. code:: bash

    {
        "id":"Policy ID",
        "insurer_policy_id":"Insurer Policy ID",
        "payment_token":"<QUOTE_TOKEN>",
        "_links":{
            "_self":{
                "href":"https:\/\/api.kasko.io\/policies\/Policy ID"
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
   "method",    "yes", "``string``", "Payment method ``invoice``."
   "provider",  "yes", "``string``", "Payment provider ``zurich_invoice``."
   "metadata.account_holder_name",  "yes", "``string``", "Account name ``Kasko``."
   "metadata.iban",  "yes", "``string``", "Account IBAN ``NO9386011117947``."
   "metadata.bic",  "yes", "``string``", "Account BIC ``12345678``."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT_TOKEN>",
            "policy_id": "<policy_id>",
            "method": "invoice",
            "provider": "zurich_invoice",
            "metadata": {
                  "account_holder_name": "Kasko",
                  "iban": "NO9386011117947",
                  "bic": "12345678"
            }
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
