REST API - Dog Surgery
======================

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
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "coverage_type", "string", "Type - smart,easy,best", "best"
   "cat_breed", "string", "A dataset_id that represents a specific cat_breed", "dai_some_id_32_chars_long_______"
   "cat_dob", "string", "Cats date of birth", "2017-11-06"
   "dental_surgery", "boolean", "Is dental surgery coverage required", "false"
   "payment_frequency", "string", "Payment frequency - monthly, quarterly, half_yearly, yearly", "half_yearly"
   "policy_start_date", "string", "Policy start date, ISO date format", "2019-12-27"
   "deductibles", "integer", "Deductible - 0, 25000", "25000"
   "cat_type", "string", "Type - home, outdoor", "home"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X GET \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u <SECRET_KEY>: \
      -d '{
        "item_id": "<ITEM_ID>",
        "touchpoint_id": "<TOUCHPOINT_ID>",
        "subscription_plan_id": "<SUBSCRIPTION_ID>",
        "data": {
            "coverage_type": "best",
            "cat_breed": "dai_some_id_32_chars_long_______",
            "cat_dob": "2019-01-01",
            "dental_surgery": true,
            "payment_frequency": "yearly",
            "policy_start_date": "2020-09-27",
            "deductibles": 25000,
            "cat_type": "home"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "token": "<TOKEN>",
        "gross_payment_amount": 11255,
        "extra_data": {
            "gross_premium": 11255,
            "premium_tax": 1797,
            "net_premium": 9458,
            "tax_rate": 0.19,
            "gross_premium_cat": 11255,
            "gross_premium_dental_surgery": 2856,
            "cat_age": 1,
            "gross_premium_dental_surgery_true": 2856,
            "gross_premium_smart": 5356,
            "gross_premium_easy": 7766,
            "gross_premium_best": 8399
        }
    }

Create Unpaid Policy Request
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "first_name", "string", "First name", "John"
   "last_name", "string", "Last name", "Tester"
   "email", "string", "Email", "test@kasko.io"
   "cat_name", "string", "Dog name", "Rex"
   "cat_gender", "string", "male or female", "male"
   "cat_with_chip", "string", "Chip type - tattoo,characteristics,chip", "tattoo"
   "cat_chip_number", "string", "Chip number", "123456789123456"
   "cat_tattoo_number", "string", "Tatoo number", "ABC123"
   "fur_color", "string", "Fur Color", "brown"
   "template", "string", "Template", "template"
   "special_characteristics", "string", "Special cat characteristics", "Special thing"
   "cat_health", "boolean", "", "true"
   "previous_insurance", "boolean", "", "false"
   "previous_insurance_name", "string", "", ""
   "previous_insurance_ended_by", "string", "", ""
   "salutation", "string", "mr / ms", "mr"
   "dob", "string", "Date of birth of dog", "2017-11-06"
   "house_number", "string", "House number", "ABC"
   "street", "string", "Street number", "DEF"
   "city", "string", "City name", "London"
   "postcode", "string", "Postal code", "12345"
   "phone", "string", "Phone number", "+999 233445566"
   "consultation", "boolean", "Is consultation needed", "false"
   "coverage_to_1000", "boolean", "", "true"
   "coverage_to_2000", "boolean", "", "true"
   "coverage_to_5000", "boolean", "", "true"
   "adnr_number", "string", "", "12"

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
                   "cat_name": "Rex",
                   "cat_gender": "male",
                   "cat_with_chip": "characteristics",
                   "cat_chip_number": "123456789123456",
                   "cat_tattoo_number": "ABC123",
                   "fur_color": "orange",
                   "template": "template",
                   "special_characteristics": "white belly",
                   "cat_health": true,
                   "previous_insurance": true,
                   "previous_insurance_name": "prevInsuranceName",
                   "previous_insurance_ended_by": "prevInsuranceEndedBy",
                   "salutation": "mr",
                   "dob": "2000-01-01",
                   "house_number": "12",
                   "street": "DEF",
                   "city": "London",
                   "postcode": "12345",
                   "phone": "+999 233445566",
                   "consultation": false,
                   "coverage_to_1000": true,
                   "coverage_to_2000": false,
                   "coverage_to_5000": false,
                   "adnr_number": "12"
                },
                "email": "test@kasko.io",
                "first_name": "First name",
                "language": "de",
                "last_name": "Last name",
                "quote_token": "<TOKEN>"
        }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

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

.. _OfferResponse:

Convert offer to policy (payment)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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
        -u <SECRET_KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
