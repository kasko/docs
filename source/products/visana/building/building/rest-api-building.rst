REST API - Visana - Building
============================


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

   building_type,string,true,"Type of the building, value (one_family,two_three_family,summer_residence,ownership)"
   basic_coverage_excess,integer,true,"Basic coverage excess, value (20000,50000,100000)"
   existing_customer,boolean,true,Existing customer
   existing_insurance,boolean,true,Existing insurance
   insured_sum,integer,true,"Insured sum, value (min:5000000, max:9999999999)"
   build_type,string,true,"Build type, value (robust,not_robust)"
   roof_type,string,true,"Roof type, value (flat,not_flat)"
   construction_year,integer,true,"Year of the construction, value (regex:/^\\d{4}$/|year:<=,this year)"
   fire_basic_coverage,boolean,true,"Fire basic coverage"
   water_basic_coverage,boolean,true,Water basic coverage
   glass_coverage,boolean,true,Glass Coverage
   glass_coverage_excess,integer,"required_if:glass_coverage,true","Glass coverage excess, value (0,20000,50000,100000)"
   costs_coverage,boolean,true,Costs coverage
   costs_coverage_insured_sum,integer,"required_if:costs_coverage,true","Costs coverage insured sum, value (min:100000, max:99999999999)"
   costs_renting_profit_coverage,boolean,true,Costs renting profit coverage
   costs_renting_profit_coverage_insured_sum,integer,"required_if:costs_renting_profit_coverage,true","Costs renting profit coverage insured sum, value (min:100000, max:99999999999)"
   costs_renting_profit_coverage_excess,integer,"required_if:costs_renting_profit_coverage,true","Costs renting profit coverage excess, value (20000,50000,100000)"
   hazards_coverage,boolean,true,Hazards coverage
   garden_coverage,boolean,true,Garden coverage
   garden_coverage_insured_sum,integer,"required_if:garden_coverage,true","Garden coverage insured sum, value (min:500000, max:99999999)"
   garden_coverage_excess,integer,"required_if:garden_coverage,true","Garden coverage excess, value (20000,50000,100000)"
   solar_system_coverage,boolean,true,Solar system coverage
   solar_system_coverage_insured_sum,integer,"required_if:solar_system_coverage,true","Solar system coverage insured sum, value (1500000,3000000,4500000,6000000)"
   equipment_materials_coverage,boolean,true,Equipment materials
   equipment_materials_coverage_insured_sum,integer,"required_if:equipment_materials_coverage,true","Equipment materials coverage insured sum, value (min: 500000, max:9999999)"
   liability_coverage,boolean,true,Liability Coverage
   liability_coverage_excess,integer,"required_if:liability_coverage,true","Liability coverage excess, value (min:20000, max:50000)"
   construction_outside_coverage,boolean,true,Construction outside coverage
   construction_outside_coverage_insured_sum,integer,"required_if:construction_outside_coverage,true","Construction outside coverage insured sum, value (min:100000, max:99999999)"
   construction_outside_coverage_excess,integer,"required_if:construction_outside_coverage,true","Construction outside coverage excess, value (min:20000, max:50000)"
   construction_outside_coverage_hazard,string,"required_if:construction_outside_coverage,true","Construction outside coverage hazard, value (fire,water,fire_water)"
   construction_outside_malicious_damage_coverage,boolean,"required_if:construction_outside_coverage,true",Construction outside malicious damage coverage
   construction_facilities_coverage,boolean,true,Construction facilities coverage
   construction_facilities_coverage_insured_sum,integer,"required_if:construction_facilities_coverage,true","Construction facilities coverage insured sum, value (min:500000, max:99999999)"
   construction_facilities_coverage_excess,integer,"required_if:construction_facilities_coverage,true","Construction facilities coverage excess, value (20000,50000,100000)"
   construction_facilities_coverage_hazard,string,"required_if:construction_facilities_coverage,true","Construction facilities coverage hazard, value (fire,fire_water)"
   duration,string,required,"Policy duration, value (P3Y,P4Y,P5Y,P6Y,P7Y,P8Y,P9Y,P10Y)"
   policy_start_date,string,true,"Policy start date, value (after:yesterday|before:+18 months, iso_date)"
   risk_address_postcode,string,true,"Risk address postcode, value (regex:/^[0-9]{4}$/|ch_postal_code)"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST https://api.eu1.kaskocloud.com/quotes \
        -u SECRET_KEY: \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -d touchpoint_id=TOUCHPOINT_ID \
        -d item_id=ITEM_ID \
        -d subscription_plan_id=SUBSCRIPTION_ID \
        -d data='{
           "building_type":"two_three_family",
           "basic_coverage_excess":20000,
           "existing_customer":true,
           "existing_insurance":true,
           "insured_sum":5000000,
           "build_type":"robust",
           "roof_type":"not_flat",
           "construction_year":2000,
           "fire_basic_coverage":true,
           "water_basic_coverage":true,
           "glass_coverage":true,
           "glass_coverage_excess":100000,
           "costs_coverage":true,
           "costs_coverage_insured_sum":100000,
           "costs_renting_profit_coverage":true,
           "costs_renting_profit_coverage_insured_sum":99999999999,
           "costs_renting_profit_coverage_excess":50000,
           "hazards_coverage":true,
           "garden_coverage":true,
           "garden_coverage_insured_sum":500000,
           "garden_coverage_excess":50000,
           "solar_system_coverage":true,
           "solar_system_coverage_insured_sum":3000000,
           "equipment_materials_coverage":true,
           "equipment_materials_coverage_insured_sum":500000,
           "liability_coverage":true,
           "liability_coverage_excess":20000,
           "construction_outside_coverage":true,
           "construction_outside_coverage_insured_sum":100000,
           "construction_outside_coverage_excess":20000,
           "construction_outside_coverage_hazard":"water",
           "construction_outside_malicious_damage_coverage":true,
           "construction_facilities_coverage":true,
           "construction_facilities_coverage_insured_sum":99999999,
           "construction_facilities_coverage_excess":20000,
           "construction_facilities_coverage_hazard":"fire",
           "duration":"P6Y",
           "policy_start_date":"2020-09-09",
           "risk_address_postcode":"1234"
         }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
       "token":"QUOTE_TOKEN",
       "gross_payment_amount":96734005,
       "extra_data":{
          "gross_premium":96734005,
          "premium_tax":4606380,
          "net_premium":92127625,
          "tax_rate":0.05,
          "basic_coverage_postcode_validation":true
       }
    }

Create Unpaid Policy Request
----------------------------
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Required", "Description"
   :widths: 20, 20, 20, 80

   policy_start_date,string,true,"Policy start date value (iso date, after:yesterday|before:+18 months)"
   duration,string,true,"Policy duration, value (P3Y,P4Y,P5Y,P6Y,P7Y,P8Y,P9Y,P10Y)"
   salutation,string,required,"Policy owner salutation, value (ms,mr)"
   house_number,string,false,House number
   street,string,false,Street name
   city,string,false,City
   postcode,string,false,"Postcode, value (regex:/^[0-9]{4}$/|ch_postal_code)"
   phone,string,true,"Phone number, value (regex:/^\\+?[0-9\\s]+$/)"
   dob,string,true,"iso date, before:18 years ago|after:100 years ago"
   sales_agent,string,true,"Sales agent, value (agent,friend,advertisement,search_engine,social_media,other)"
   agent_details,string,"required_if:sales_agent,agent","Agent details"
   save_for_later_email,string,false,"Save for later email, value (regex:/\\S+@\\S+\\.\\S+/)"
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
              "policy_start_date":"2020-09-09",
              "duration": "P3Y",
              "salutation": "mr",
              "dob": "1984-12-29",
              "phone": "+41777777777",
              "house_number": "1234",
              "street": "Test Stasse",
              "city": "Vessy",
              "postcode": "1234",
              "sales_agent":"search_engine",
              "agent_details":"sales_agent",
              "save_for_later_email":"test@email.com",
              "risk_address_postcode":"1234",
              "no_damages": true,
              "flexible_cancellation": false,
              "comments": "test comment",
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
