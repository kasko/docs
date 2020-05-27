REST API - Private Liability
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

   existing_customer,boolean,true,Does the customer have health insurance with Visana
   existing_insurance,boolean,true,Does the customer have an existing insurance (private liability & content)
   persons,string,true,"How many people are living in the property, value (single, family)"
   ownership,string,true,"Ownership of the property, value (owner, renter)"
   more_than_one_person,boolean,true,Module to insure additional people in the same property
   person_count,integer,"required_if:more_than_one_person,true","The amount of people to insure additionally, value (min, max:3)"
   renters_damage,integer,"required_if:ownership,renter","Deductible for renter damages, value (0, 20000)"
   insured_sum,integer,true,"Insured sum of renter damages, value (500000000, 1000000000)"
   optional_modules1,boolean,true,Insure optional module: liability of driving a car that's not owned by you
   optional_modules2,boolean,true,Insure optional module: model aircraft
   optional_modules3,boolean,true,Insure optional module: participation of horse riding sport activities
   optional_modules4,boolean,true,Insure optional module: damages of riding a horse that's not owned by you
   optional_modules4_guarantee,string,"required_if:optional_modules4,true","Insured sum (damages of the horse), value (10_20, 10_30, 20_20, 20_30, 30_30, 30_40, 30_50)"
   optional_modules4_equestrian,boolean,"required_if:optional_modules4,true",Insure horse riding event
   duration,string,true,"Policy duration, value (P3Y, P4Y, P5Y, P6Y, P7Y, P8Y, P9Y, P10Y)"
   policy_start_date,string,true,"Policy start date, value (iso_date, after:yesterday, before:+18 months)"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u <SECRET KEY>: \
      -d '{
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID>",
        "subscription_plan_id": "<SUBSCRIPTION PLAN ID>",
        "data": {
          "existing_customer":false,
          "existing_insurance":false,
          "persons":"single",
          "ownership":"owner",
          "more_than_one_person":false,
          "person_count":1,
          "renters_damage":20000,
          "insured_sum":500000000,
          "optional_modules1":false,
          "optional_modules2":false,
          "optional_modules3":false,
          "optional_modules4":false,
          "optional_modules4_guarantee":"10_20",
          "optional_modules4_equestrian":false,
          "duration":"P3Y",
          "policy_start_date":"2019-09-06"
        }
    }'


Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "token": <Quote Token>,
        "gross_payment_amount": 22995,
        "extra_data": {
            "gross_premium": 22995,
            "premium_tax": 1095,
            "net_premium": 21900,
            "tax_rate": 0.05,
            "suggested_insured_sum_hr": 0,
            "lock_change": 0,
            "private_liability_gross_premium": 22995,
            "yearly_private_liability_gross_premium": 7665
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
   sales_agent,integer,true,Agent Number
   agent_details,string,false,Agent Details
   no_damages,boolean,true,Opt-in confirming that the customers have not been rejected/cancelled  by other insurance companies or received special conditions due to damages
   flexible_cancellation,boolean,true,Does the customer want to have flexible cancellation term for his/her policy


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
              "existing_customer": false,
              "existing_insurance": false,
              "salutation": "mr",
              "dob": "1984-12-29",
              "phone": "+41777777777",
              "house_number": "1234",
              "street": "Test Stasse",
              "city": "Vessy",
              "postcode": "1234",
              "sales_agent": 5,
              "no_damages": true,
              "flexible_cancellation": false
          },
          "email": "test@kasko.io",
          "first_name": "First name",
          "language": "de",
          "last_name": "Last name",
          "quote_token": "quote_token"
    }'

Example Response
~~~~~~~~~~~~~~~~
.. _OfferResponse:

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
   "metadata.iban",  "yes", "``string``", "Agreed IBAN ."
   "metadata.bic",  "yes", "``string``", "Agreed BIC."

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
