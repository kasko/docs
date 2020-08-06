REST API - Roland - Haus und Grund
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
   "previous_insurance", "boolean", "", "true"
   "previous_damages", "integer", "Required if previous insurance true", "1"
   "policy_start_date", "string", "Policy start date", "2017-11-06"
   "rental_number", "integer", "Required if rental module true", "1"
   "living_module", "boolean", "Living module", "true"
   "rental_module", "boolean", "Rental module", "false"

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
            "living_module": "true",
            "policy_start_date": "2020-08-06",
            "previous_damages": "1",
            "previous_insurance": "false",
            "rental_module": "true",
            "rental_number": "1"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript
      {
        "extra_data": {
            "gross_premium": 10154,
            "net_premium": 8533,
            "premium_tax": 1621,
            "tax_rate": 0.19
        ],
        "gross_payment_amount": 10154,
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
   "dob", "string", "Date of birth", "2000-11-06"
   "phone", "string", "Phone number", "+999 233445566"
   "gender", "string", "Ms or Mr", "mr"
   "sepa_payment", "boolean", "Sepa payment", "true"
   "existing_living_module", "boolean", "Prefilled living address as personal address", "true"
   "previous_insurance", "boolean", "Previous Insurance", "true"
   "living_module", "boolean", "Insurance on living address", "true"
   "rental_module", "boolean", "Insurance on rental addresses", "false"
   "rental_number", "integer", "Number of rental addresses", "4"
   "filtered_duplex_houses", "array", "Selected rental addresses for insurance", "4"
   "house_number", "string", "House number", "ABC"
   "street", "string", "Street number", "DEF"
   "city", "string", "City name", "London"
   "postcode", "string", "Postal code", "12345"
   "description", "string", "Description of address", "Near park"
   "living_selected", "boolean", "If living address needs insurance", "false"
   "duplex_selected", "boolean", "If rental addresses needs insurance", "true"
   "previous_insurance_policy", "string", "Previous insurance information", "QQ123456C"
   "personal_details_house_number", "string", "Current house number", "ABC"
   "personal_details_street", "string", "Current street number", "DEF"
   "personal_details_city", "string", "Current city name", "London"
   "personal_details_postcode", "string", "Current postal code", "12345"
   "previous_insurance_additional", "string", "Previous insurance company", "Jurpartner"
   "duplex_house_number", "string", "Rental house number", "ABC"
   "duplex_street", "string", "Rental street number", "DEF"
   "duplex_city", "string", "Rental city name", "London"
   "duplex_postcode", "string", "Rental postal code", "12345"
   "duplex_description", "string", "Description of rental address", "Near park"

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
                "city": "Entenhausen",
                "description": "",
                "dob": "1992-02-02",
                "existing_living_module": true,
                filtered_duplex_houses: [
                  "0": {
                      "duplex_city": "Ort eins",
                      "duplex_description": "",
                      "duplex_house_number": "1",
                      "duplex_postcode": "12121",
                      "duplex_selected": true,
                      "duplex_street": "Erster Weg"
                  },
                  "1": {
                      "duplex_city": "Ort zwei",
                      "duplex_description": "",
                      "duplex_house_number": "2",
                      "duplex_postcode": "13131",
                      "duplex_selected": true,
                      "duplex_street": "Zweiter Weg"
                  }
                ],
                "gender": "ms",
                "house_number: "13",
                "living_module: true,
                "living_selected": true,
                "personal_details_city": "",
                "personal_details_house_number": "",
                "personal_details_postcode": "",
                "personal_details_street": "",
                "phone": "234234",
                "postcode": "13131",
                "previous_insurance": false,
                "rental_module": true,
                "rental_number": 3,
                "sepa_payment": true,
                "street": "Entengasse"
            },
            "email": "test@kasko.io",
            "first_name": "Daisy",
            "language": "de",
            "last_name": "Duck",
            "media": {},
            "metadata": {
                "tan": "69682090-047-0000038903"
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