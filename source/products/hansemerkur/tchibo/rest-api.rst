REST API - Tchibo Dog Surgery
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

   "coverage_type", "string", "Type - smart_2000, top_5000", "smart_2000"
   "dog_breed", "string", "Dog breed", "boxer"
   "dog_dob", "string", "Dogs date of birth", "2017-11-06"
   "dental_surgery", "boolean", "Is dental surgery coverage required", "false"
   "payment_frequency", "string", "yearly, half_yearly, quarterly, monthly", "half_yearly"
   "policy_start_date", "string", "Policy start date", "2019-12-27"
   "deductibles", "string", "Type - 0, 25000", "25000"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X GET \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u <SECRET KEY>: \
      -d '{
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID>",
        "subscription_plan_id": "<SUBSCRIPTION PLAN ID>",
        "data": {
            "coverage_type": "top_5000",
            "dog_breed": "boxer",
            "dog_dob": "2019-01-01",
            "dental_surgery": true,
            "payment_frequency": "yearly",
            "policy_start_date": "2019-12-27",
            "deductibles": "25000"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

      {
        "extra_data": {
            "gross_premium": 48123,
            "net_premium": 40439,
            "premium_tax": 7684,
            "tax_rate": 0.19,
            "gross_premium_dental_surgery": 2281,
            "gross_premium_dog": 45842,
            "dog_age": 0
        ],
        "gross_payment_amount": 48123,
        "token": "encrypted_token"
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
   "dog_name", "string", "Dog name", "Rex"
   "dog_gender", "string", "male or female", "male"
   "dog_with_chip", "boolean", "Does dog has a chip?", "true"
   "dog_chip_number", "string", "Chip number", "123456789123456"
   "dog_tattoo_number", "string", "Tatoo number", "ABC123"
   "dog_health", "boolean", "", "true"
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
   "consultation", "boolean", "Is consulation needed", "false"
   "coverage_to_2000", "boolean", "", "true"
   "coverage_to_5000", "boolean", "", "true"
   "coverage_extended", "boolean", "", "true"
   "adnr_number", "string", "", "12"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'Content-Type: application/json' \
        -u <SECRET KEY>: \
        -d '{
            "data": {
               "dog_name": "Rex",
               "dog_gender": "male",
               "dog_with_chip": true,
               "dog_chip_number": "123456789123456",
               "dog_tattoo_number": "ABC123",
               "dog_health": true,
               "previous_insurance": false,
               "previous_insurance_name": "",
               "previous_insurance_ended_by": "",
               "salutation": "mr",
               "dob": "2000-01-01",
               "house_number": "12",
               "street": "DEF",
               "city": "London",
               "postcode": "12345",
               "phone": "+999 233445566",
               "consultation": false,
               "coverage_to_2000": false,
               "coverage_to_5000": false,
               "coverage_extended": true,
               "adnr_number": "12"
            },
            "email": "test@kasko.io",
            "first_name": "First name",
            "language": "de",
            "last_name": "Last name",
            "quote_token": "quote_token",
            "metadata": {
                "agent_company_name": "Company name",
                "agent_email": "test@kasko.io",
                "agent_first_name": "Firstname",
                "agent_last_name": "Lastname",
                "agent_number": "12345",
                "agent_phone": "49711111",
                "agent_salutation": "Mr",
                "reference_number": "123"
            }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
      "id": "Insurer Policy ID",
      "insurer_policy_id": "Policy ID",
      "payment_token": "TOKEN",
      "_links": {
        "_self": {
          "href": "https:\/\/api.kasko.io\/policies\/[Insurer Policy ID]"
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
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
