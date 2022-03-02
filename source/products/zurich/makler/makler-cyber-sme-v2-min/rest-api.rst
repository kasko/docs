REST API - Zurich - Cyber SME V2 min (Makler/ZEP)
=================================================

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint .**

V2 Header
----------

The API requests must use the V2 header as show in the examples below:

``Accept: application/vnd.kasko.v2+json``

Quote Data
^^^^^^^^^^
Query string data appended to the quote request

.. csv-table::
   :header: "Name", "Type", "Example Value"
   :widths: 20, 20, 20

   "cover", "string", "basis"
   "card_payment", "boolean", "true"
   "payment_frequency", "string", "yearly"
   "second_company", "boolean", "false"
   "cyber_fraud", "boolean", "true"
   "data_backup", "boolean", "true"
   "data_privacy", "boolean", "true"
   "deductible", "integer", "100000"
   "distributor", "string", "makler_de"
   "foreign_subsidiary", "boolean", "false"
   "insured_sum", "integer", "50000000"
   "password_update", "boolean", "true"
   "sector", "string", "dai_some_id_32_chars_long_______"
   "second_company_sector", "string|optional", "dai_some_id_33_chars_long_______"
   "security_issues", "boolean", "false"
   "security_software", "boolean", "false"
   "start_date", "string", "2021-01-01"
   "tax_country", "string", "at"
   "turnover", "integer", "24999900"
   "second_company_turnover", "integer", "199999900"
   "cyber_circumstances", "boolean", "true"
   "duration", "string", "P1Y"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X GET \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u SECRET_KEY: \
      -d '{
        "item_id": "ITEM_ID",
        "touchpoint_id": "TOUCHPOINT_ID",
        "subscription_plan_id": "SUBSCRIPTION_ID",
        "data": {
            "cover": "basis",
            "card_payment": true,
            "payment_frequency": "yearly",
            "second_company": false,
            "cyber_fraud": false,
            "data_backup": true,
            "data_privacy": true,
            "deductible": 100000,
            "distributor": "makler_de",
            "foreign_subsidiary": true,
            "insured_sum": 50000000,
            "password_update": true,
            "sector": "dai_some_id_32_chars_long_______",
            "security_issues": true,
            "security_software": true,
            "start_date": "2021-02-09",
            "tax_country": "de",
            "turnover": 24999900,
            "cyber_circumstances": true,
            "duration": "P1Y"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "token": "QUOTE_TOKEN",
        "gross_payment_amount": 75581,
        "extra_data": {
            "gross_premium": 75581,
            "premium_tax": 7490,
            "net_premium": 68091,
            "tax_rate": 0.11,
            "flow": "default",
            "policy_end_date": "2022-01-09",
            "crisis_management": 1000000,
            "emergency_costs": 2500000,
            "digital_asset_replacement": 10000000,
            "hardware_damage": 2500000,
            "system_recovery": 50000000,
            "business_interruption": 25000000,
            "security_imrovement": 500000,
            "cyber_extortion": 5000000,
            "pci": 25000000,
            "breach_costs": 50000000,
            "regulatory_fines": 10000000,
            "security_liability": 50000000,
            "internet_media_liability": 25000000,
            "cyber_terrorism": 50000000,
            "cyber_crime": 0
        }
    }

Create Unpaid Policy Request
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Example Value"
   :widths: 20, 20, 20

    "social_engineering_fraud", "boolean", "true"
    "bank_transfer_policy", "boolean", "true"
    "security_issues_description", "string", "Issue description"
    "security_issues_damage", "integer", "50"
    "security_issues_actions", "string", "Actions taken"
    "company_name", "string", "KASKO"
    "company_legal_form", "string", "LegalForm"
    "company_street", "string", "Test St."
    "company_house_number", "string", "57a-1"
    "company_postcode", "string", "12345"
    "company_city", "string", "Riga"
    "company_website", "string", "www.kasko.io"
    "salutation", "string", "ms"
    "phone", "string", "+999 233445566"
    "email", "string", "test@kasko.io"
    "agent_email", "string", "testAgent@kasko.io"
    "agent_company_name", "string", "agentCompanyName"
    "agent_salutation", "string", "mr"
    "agent_first_name", "string", "Name"
    "agent_last_name", "string", "lastName"
    "agent_number", "string", "123123123123"
    "svb_number", "string", "34343434343"
    "agent_phone", "string", "+999 233445566"
    "password_update_question", "string", "passwordUpdateQuestion"
    "data_backup_question", "string", "dataBackupQuestion"
    "cyber_circumstances_individual", "string", "circumstancesIndividual"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

        curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'Content-Type: application/json' \
        -u <SECRET_KEY>: \
        -d '{
            "data": {
               "social_engineering_fraud": false,
               "bank_transfer_policy": false,
               "security_issues_description": "Issue description",
               "security_issues_damage": 59,
               "security_issues_actions": "Actions Taken",
               "company_name": "KASKO",
               "company_legal_form": "LegalForm",
               "company_street": "Test St",
               "company_house_number": "57a",
               "company_postcode": "12345",
               "company_city": "Riga",
               "company_website": "www.kasko.io",
               "salutation": "mr",
               "phone": "+496123123123",
               "email": "test@kasko.io",
               "agent_email": "testAgent@kasko.io",
               "agent_company_name": "Agent Company",
               "agent_salutation": "mr",
               "agent_first_name": "Agent Name",
               "agent_last_name": "Agent Last Name",
               "agent_number": "123123132",
               "svb_number": "1231",
               "agent_phone": "+496123123123",
               "password_update_question": "PW Update Question",
               "data_backup_question": "Data Backup Question",
               "cyber_circumstances_individual": "Cyber circumstances question"
            },
            "email": "test@kasko.io",
            "first_name": "First Name",
            "language": "de",
            "last_name": "Last Name",
            "quote_token": "QUOTE_TOKEN"
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "id": "POLICY_ID",
        "insurer_policy_id": "INSURER_POLICY_ID",
        "payment_token": "PAYMENT_TOKEN",
        "_links": {
            "_self": {
                "href": "https:\/\/api.eu1.kaskocloud.com\/policies\/POLICY_ID"
            }
        }
    }

.. note::  This product is using a feature called ``Manual underwriting``. This means that a policy can be marked with this status. If this is the case, ``PAYMENT TOKEN`` won't be present in the policy response. In order to find this token, distributor has to first approve the policy in the self service dashboard and make an API call to see the created unpaid policy data. Payment token will be available there. If the policy is not marked with "Manual Underwriting", payment token will be available right away in the policy response.

Get unpaid policy data (offer)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    curl -X GET \
      'https://api.kasko.io/offers/<POLICY_ID>' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u <SECRET_KEY>:

Convert offer to policy (payment)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create a policy you should convert offer to policy. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by OfferResponse."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by OfferResponse."
   "method",    "yes", "``string``", "Payment method ``distributor``."
   "provider",  "yes", "``string``", "Payment provider ``distributor``."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <SECRET_KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from Policy response. After payment is made, policy creation is asynchronous.
