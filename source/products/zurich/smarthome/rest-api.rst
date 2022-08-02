================
Zurich Smarthome
================

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in `Quote`_, `Offer`_, `Payment`_ requests.

.. _Quote:

Quote Data
-------------------------------------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "start_date", "string", "Policy start date. ISO Date format", "yyyy-mm-dd"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl -X post  https://api.kasko.io/quotes \
      -u <sk_test_SECRET_KEY>: \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -d touchpoint_id=TOUCHPOINT_ID \
      -d item_id=ITEM_ID \
      -d subscription_plan_id=SUBSCRIPTION_ID \
      -d data='{
          "start_date": "2019-11-15"
        }'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

   {
	   "token":"QUOTE_TOKEN",
	   "gross_payment_amount":1499,
	   "extra_data": {
		   "billing_cycles":36
	   }
   }

Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "agent_company",                      "string", "Name of the Agent Company. Max: 255 characters", "Test Agent Company Name"
   "agent_salutation",                   "string", "Agent salutation. Either ms or mr", "mr"
   "agent_first_name",                   "string", "Agents first name", "Test Name"
   "agent_last_name",                    "string", "Agents last name", "Test Last Name"
   "agent_number",                       "string", "Agents numbers. Min: 9, Max: 9", "111111111"
   "agent_email",                        "string", "Agents Email. Max: 255 characters", "example.agent.email@kasko.io"
   "agent_phone",                        "string", "Agents Phone number", "+11111111"
   "agent_address_street",               "string", "Street name of the agents address", "test st."
   "agent_address_house_number",         "string", "House number of the agents address", "5A"
   "agent_address_postcode",             "string", "Postcode of the agents address. Must be 5 characters long", "12345"
   "agent_address_city",                 "string", "City of the agents address", "London"
   "contract_number",                    "string", "Contract number, must be 12 characters long", "123123123123"
   "contract_start",                     "string", "Start of the contract. ISO Date format", "yyyy-mm-dd"
   "contract_due_date",                  "string", "Contract due date. ISO Date format", "yyyy-mm-dd"
   "contract_duration",                  "string", "Contract duration, always 3", "3"
   "contract_end",                       "string", "Contract end date. ISO Date format", "yyyy-mm-dd"
   "subscription_plan_id",               "string", "Subscription id from the following two: Monthly frequency - sp_1f244f3968bf23e3ca3c0e87f9bfd, Yearly frequency - sp_a12bc925e58db408f4c9050d1fd69", "sp_a12bc925e58db408f4c9050d1fd69"
   "salutation",                         "string", "Salutation, either mr or ms", "ms"
   "dob",                                "string", "Date of birth, Must be 18 or older and 100 or younger. ISO Date format", "yyyy-mm-dd"
   "phone",                              "string", "Phone number", "+11111111"
   "street",                             "string", "Street", "test street"
   "house_number",                       "string", "House Number", "67"
   "postcode",                           "string", "Postcode. Must be 5 characters long", "12345"
   "city",                               "string", "City", "London"
   "property_address_is_client_address", "bool", "Is property address client address", "true"
   "property_address_street",            "string", "Property address street. Required If: property_address_is_client_address = false", "test street"
   "property_address_house_number",      "string", "Property address house number. Required If: property_address_is_client_address = false", "54"
   "property_address_postcode",          "string", "Property address postcode, must be 5 characters long. Required If: property_address_is_client_address = false", "12345"
   "property_address_city",              "string", "Property address city. Required If: property_address_is_client_address = false", "Riga"
   "delivery_address",                   "string", "Delivery address, either client or property. Required If: property_address_is_client_address = false", "property"
   "emergency_contact_count",            "integer", "Emergency contact count. From 0 to 3", "2"
   "emergency_contact_1_salutation",     "string", "Emergency contact salutation, either mr or ms. Required If: emergency_contact_count = 1, 2, 3", "ms"
   "emergency_contact_1_first_name",     "string", "Emergency contact name. Max 255 characters. Required If: emergency_contact_count = 1, 2, 3", "test_emergency_contact_name"
   "emergency_contact_1_last_name",      "string", "Emergency contact last name. Max 255 characters. Required If: emergency_contact_count = 1, 2, 3", "test_emergency_contact_last_name"
   "emergency_contact_1_phone",          "string", "Emergency contact phone number. Required If: emergency_contact_count = 1, 2, 3", "+11111111"
   "emergency_contact_2_salutation",     "string", "Emergency contact salutation, either mr or ms. Required If: emergency_contact_count = 2, 3", "ms"
   "emergency_contact_2_first_name",     "string", "Emergency contact name. Max 255 characters. Required If: emergency_contact_count = 2, 3", "test_emergency_contact_name"
   "emergency_contact_2_last_name",      "string", "Emergency contact last name. Max 255 characters. Required If: emergency_contact_count = 2, 3", "test_emergency_contact_last_name"
   "emergency_contact_2_phone",          "string", "Emergency contact phone number. Required If: emergency_contact_count = 2, 3", "+11111111"
   "emergency_contact_3_salutation",     "string", "Emergency contact salutation, either mr or ms. Required If: emergency_contact_count = 3", "mr"
   "emergency_contact_3_first_name",     "string", "Emergency contact name. Max 255 characters. Required If: emergency_contact_count = 3", "test_emergency_contact_name"
   "emergency_contact_3_last_name",      "string", "Emergency contact last name. Max 255 characters. Required If: emergency_contact_count = 3", "test_emergency_contact_last_name"
   "emergency_contact_3_phone",          "string", "Emergency contact phone number. Required If: emergency_contact_count = 3", "+11111111"

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
                   "agent_company": "test_agent_company",
                   "agent_salutation": "mr",
                   "agent_first_name": "agent_first_name",
                   "agent_last_name": "agent_last_name",
                   "agent_number": "123456789",
                   "agent_email": "example@agent.io",
                   "agent_phone": "+11111111",
                   "agent_address_street": "test street",
                   "agent_address_house_number": "12",
                   "agent_address_postcode": "12345",
                   "agent_address_city": "Riga",
                   "contract_number": "123456789123",
                   "contract_start": "2019-01-01",
                   "contract_due_date": "2019-10-10",
                   "contract_duration": "3",
                   "contract_end": "2021-12-31",
                   "subscription_plan_id": "sp_1f244f3968bf23e3ca3c0e87f9bfd",
                   "salutation": "ms",
                   "dob": "1990-01-01",
                   "phone": "+11122233",
                   "street": "test street",
                   "house_number": "54",
                   "postcode": "09876",
                   "city": "London",
                   "property_address_is_client_address": true,
                   "emergency_contact_count": "1",
                   "emergency_contact_1_salutation": "ms",
                   "emergency_contact_1_first_name": "emergency_contact_name",
                   "emergency_contact_1_last_name": "emergency_contact_last_name",
                   "emergency_contact_1_phone": "+12312323"
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
      "id":"POLICY_ID",
      "insurer_policy_id":"INSURER_POLICY_ID",
      "payment_token":"",
      "_links": {
        "_self": {
          "href":"https:\/\/api.kasko.io\/policies\/POLICY_ID"
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
        -u sk_test_SECRET_KEY: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "PAYMENT_TOKEN",
            "policy_id": "POLICY_ID",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
