HanseMerkur - Dog Surgery Insurance
============================================

Product Information
^^^^^^^^^^^^^^^^^^^

.. csv-table::
   :widths: 25, 75

   "Name",      "HanseMerkur - Dog Surgery"
   "Insurer",   "HanseMerkur"
   "Region",    "Germany"
   "Languages", "``DE``"

Product IDs
^^^^^^^^^^^

.. csv-table::
   :header: "Item Title", "Item ID"

   "HanseMerkur - Dog Surgery", "``item_62d714325b7b00d936f66d485cf``"

Quote Data
^^^^^^^^^^
Query string data appended to the quote request

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "coverage_type", "string", "Type - smart_1000, smart_2000, top_5000", "smart_2000"
   "dog_breed", "string", "Dog breed", "boxer"
   "dog_dob", "string", "Dogs date of birth", "2017-11-06"
   "dental_surgery", "boolean", "Is dental surgery coverage required", "false"
   "payment_frequency", "string", "yearly, half_yearly, quarterly, monthly", "half_yearly"
   "policy_start_date", "string", "Policy start date", "2019-12-27"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X GET \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -d '{
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID>",
        "subscription_plan_id": "<SUBSCRIPTION PLAN ID>",
        "key": "<PUBLIC KEY>"
        "data": {
            "coverage_type": "top_5000",
            "dog_breed": "boxer",
            "dog_dob": "2019-01-01",
            "dental_surgery": true,
            "payment_frequency": "yearly",
            "policy_start_date": "2019-12-27"
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
        ],
        "gross_payment_amount": 48123,
        "token": "encrypted_token",
      }


Policy Data
^^^^^^^^^^^
Query string data appended to the policy request

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "first_name", "string", "First name", ""
   "last_name", "string", "Last name", ""
   "email", "string", "Email", ""
   "dog_name", "string", "Dog name", ""
   "dog_gender", "string", "male or female", ""
   "dog_with_chip", "boolean", "Does dog has a chip?", "true"
   "dog_chip_number", "string", "Chip number", "123456789123456"
   "dog_tattoo_number", "string", "Tatoo number", "ABC123"
   "dog_health", "boolean", "", "true"
   "previous_insurance", "boolean", "", "true"
   "previous_insurance_name", "string", "", ""
   "previous_insurance_ended_by", "string", "", ""
   "salutation", "string", "mr / ms", ""
   "dob", "string", "Date of birth of dog", "2017-11-06"
   "house_number", "string", "House number", "ABC"
   "street", "string", "Street number", "DEF"
   "city", "string", "City name", ""
   "postcode", "string", "Postal code", "12345"
   "phone", "string", "Phone number", "+999 233445566"
   "consultation", "boolean", "Is consulation needed", "false"
   "coverage_to_1000", "boolean", "", "true"
   "coverage_to_2000", "boolean", "", "true"
   "coverage_to_5000", "boolean", "", "true"
   "adnr_number", "string", "", "12"

Integration methods
^^^^^^^^^^^^^^^^^^^

.. toctree::
   :maxdepth: 1

   webapp
