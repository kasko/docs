REST API - Visana - Hausrat
===========================


.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint .**

V2 Header
----------

The API requests must use the V2 header as show in the examples below:

``Accept: application/vnd.kasko.v2+json``

Quote Data
----------
Query string data appended to the quote request.

An industry type must be selected but it can have more than one.

.. csv-table::
   :header: "Name", "Type", "Required", "Description"
   :widths: 20, 20, 20, 80

   existing_customer,boolean,true,Does the customer have health insurance with Visana
   existing_insurance,boolean,true,Does the customer have an existing insurance
   adults,string,true,"Amount of adults, value (1,1_5,2,2_5,3,3_5,4,4_5,5,5_5,6,6_5,7,7_5,8,8_5,9)"
   rooms,string,true,"Amount of rooms, value (1,1_5,2,2_5,3,3_5,4,4_5,5,5_5,6,6_5,7,7_5,8,8_5,9)"
   insured_sum_hr,integer,true,"Amount of rooms, value (min:2000000, max:999999900)"
   massive_design_type,boolean,true,"massive_design_type"
   house_type,string,true,"Type of house value (apartment, detached, holiday)"
   main_modules1,boolean,true,"Main module 1"
   main_modules1_deductible,integer,"required_if:main_modules1,true","Main modules 1 deductible Value (20000,50000,100000)"
   main_modules2,boolean,true,"Main module 2"
   main_modules3,boolean,true,"Main module 3"
   insure_glass,boolean,true,"Insure glass"
   insure_glass_furniture,boolean,"required_if:insure_glass,true","Insure glass furniture"
   insure_glass_building,boolean,"required_if:insure_glass,true","Insure glass building"
   insure_glass_building_deductible,integer,"required_if:insure_glass_building,true","Insure glass building deductible Value (0,20000,50000,100000)"
   private_use,boolean,true,"Private use"
   additional_theft_abroad,boolean,true,"Additional theft abroad"
   additional_theft_bike,integer,true,"Additional theft bike Value (0,300000,500000,1000000,1500000,2000000)"
   additional_theft_luggage,boolean,"required_if:additional_theft_abroad,true","Additional theft luggage"
   additional_theft_abroad_sum,integer,"required_if:additional_theft_abroad,true","Additional theft abroad sum, value(200000,300000,400000,500000,600000,700000,800000,900000,1000000,1100000,1200000,1300000,1400000,1500000,1600000,1700000,1800000,1900000,2000000)"
   household_contents,boolean,true,"Household contents"
   household_contents_sum,integer,"required_if:household_contents,true","Household contents, value (200000,300000,400000,500000)"
   household_contents_deductible,integer,"required_if:household_contents,true","Household contents, value (20000,50000,100000)"
   lock_change,boolean,true,"Lock change"
   duration,string,true,"Policy duration, value (P3Y, P4Y, P5Y, P6Y, P7Y, P8Y, P9Y, P10Y)"
   policy_start_date,string,true,"Policy start date, value (iso_date, after:yesterday, before:+18 months)"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST https://api.eu1.kaskocloud.com/policies \
        -u SECRET_KEY:: \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -d touchpoint_id=TOUCHPOINT_ID \
        -d item_id=ITEM_ID \
        -d subscription_plan_id=SUBSCRIPTION_ID \
        -d data='{
           "existing_customer":false,
           "existing_insurance":false,
           "adults":"1",
           "rooms":"1_5",
           "insured_sum_hr":999999900,
           "massive_design_type":true,
           "house_type":"apartment",
           "main_modules1":true,
           "main_modules1_deductible":50000,
           "main_modules2":true,
           "main_modules3":true,
           "insure_glass":true,
           "insure_glass_furniture":true,
           "insure_glass_building":true,
           "insure_glass_building_deductible":20000,
           "private_use":true,
           "additional_theft_abroad":true,
           "additional_theft_bike":300000,
           "additional_theft_luggage":true,
           "additional_theft_abroad_sum":300000,
           "household_contents":false,
           "household_contents_sum":500000,
           "household_contents_deductible":100000,
           "lock_change":true,
           "duration":"P3Y",
           "policy_start_date":"2020-09-06"
         }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
       "token":"QUOTE_TOKEN",
       "gross_payment_amount":5894910,
       "extra_data":{
          "gross_premium":5894910,
          "premium_tax":280710,
          "net_premium":5614200,
          "tax_rate":0.05,
          "suggested_insured_sum_hr":39300,
          "lock_change":15000,
          "yearly_gross_premium":1964970
       }
    }

Create Unpaid Policy Request
----------------------------
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Required", "Description"
   :widths: 20, 20, 20, 80

   existing_customer,boolean,true,Does the customer have health insurance with Visana
   existing_insurance,boolean,true,Does the customer have an existing insurance (private liability & content)
   salutation,string,true,"Salutation of the policyholder, value (ms, mr)"
   dob,string,true,"Date of birth od the policyholder, value (iso_date, before:18 years ago, after:100 years ago)"
   phone,string,true,"Phone number of the policyholder, value (regex:/^\\+?[0-9\\s]+$/)"
   house_number,string,false,House number of the address
   street,string,true,Street name of the address
   city,string,true,City name of the address
   postcode,string,true,"Postcode of the address, value (regex:/^[0-9]{4}$/, ch_postal_code)"
   risk_address_house_number,string,false,Risk address house number
   risk_address_street,string,true,Risk address street
   risk_address_city,string,required,Risk address city
   risk_address_postcode,string,true,"Postcode of the address, value (regex:/^[0-9]{4}$/, ch_postal_code)"
   sales_agent,integer,true,Agent Number
   agent_details,string,false,Agent Details
   no_damages,boolean,true,Opt-in confirming that the customers have not been rejected/cancelled  by other insurance companies or received special conditions due to damages
   flexible_cancellation,boolean,true,Does the customer want to have flexible cancellation term for his/her policy
   comments,string,false,Any additional comments

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

  curl -X POST \
    'https://api.eu1.kaskocloud.com/policies' \
    -u SECRET_KEY: \
    -H 'Accept: application/vnd.kasko.v2+json' \
    -H 'Content-Type: application/json' \
    -d '{
    "data": {
              "existing_customer": false,
              "existing_insurance": false,
              "salutation": "mr",
              "dob": "1984-12-29",
              "phone": "+41777777777",
              "house_number": "1234",
              "street": "Test Stasse",
              "city": "Vessy",
              "postcode": "1234",
              "risk_address_house_number":"12a",
              "risk_address_street":"test street",
              "risk_address_city":"City",
              "risk_address_postcode":"1234",
              "sales_agent": 5,
              "no_damages": true,
              "flexible_cancellation": false,
              "comments": "test comment"
    },
    "quote_token":"QUOTE_TOKEN",
    "first_name": "Test",
    "last_name": "Person",
    "email": "test@kasko.io",
    "language": "de"
    }'

Example Response
~~~~~~~~~~~~~~~~
.. _OfferResponse:

.. code:: javascript

    {
       "id":"POLICY_ID",
       "insurer_policy_id":"INSURER_POLICY_ID",
       "payment_token":"PAYMENT_TOKEN",
       "_links":{
          "_self":{
             "href":"https:\/\/api.eu1.kaskocloud.com\/policies\/POLICY_ID"
          }
       }
    }

Convert offer to policy (payment)
---------------------------------

To create a policy you should convert offer to policy. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80


   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by OfferResponse_."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by OfferResponse_."
   "method",    "yes", "``string``", "Payment method ``invoice``."
   "provider",  "yes", "``string``", "Payment provider ``invoice``."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.eu1.kaskocloud.com/payments \
        -X POST \
        -u SECRET_KEY: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "PAYMENT_TOKEN",
            "policy_id": "POLICY_ID",
            "method": "distributor",
            "provider": "distributor"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
