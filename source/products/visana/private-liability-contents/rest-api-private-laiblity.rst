REST API - Private Liablity
========

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


**Name**|**Type**|**Required **|**Description**
:-----:|:-----:|:-----:|:-----:
existing\_customer|boolean|true|Does the customer have health insurance with Visana
existing\_insurance|boolean|true|Does the customer have an exisiting insurance (private liability & content)
persons|string|true|How many people are living in the property, value (single, family)
ownership|string|true|Ownership of the property, value (owner, renter)
more\_than\_one\_person|boolean|true|Module to insure additional people in the same property
person\_count|integer|required\_if:more\_than\_one\_person,true|The amount of people to insure additionally, value (min, max:3)
renters\_damage|integer|required\_if:ownership,renter|Deductible for renter damanges, value (0, 20000)
insured\_sum|integer|true|Insured sum of renter damages, value (500000000, 1000000000)
optional\_modules1|boolean|true|Insure optional module: liability of driving a car that's not owned by you
optional\_modules2|boolean|true|Insure optional module: model aircraft
optional\_modules3|boolean|true|Insure optional module: participation of horseriding sport activities
optional\_modules4|boolean|true|Insure optional module: damages of riding a horse that's not owned by you
optional\_modules4\_guarantee|string|required\_if:optional\_modules4,true|Insured sum (damages of the horse), value (10\_20, 10\_30, 20\_20, 20\_30, 30\_30, 30\_40, 30\_50)
optional\_modules4\_equestrian|boolean|required\_if:optional\_modules4,true|Insure horse riding event
duration|string|true|Policy duration, value (P3Y, P4Y, P5Y, P6Y, P7Y, P8Y, P9Y, P10Y)
policy\_start\_date|string|true|Policy start date, value (iso\_date, after:yesterday, before:+18 months)

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
            "industry_architects_engineers": true,
            "industry_construction": false,
            "industry_regulator": false,
            "industry_consultancy": false,
            "industry_educational_institution": false,
            "industry_health_services": false,
            "industry_retail_wholesale": false,
            "industry_financial_service": false,
            "industry_property": false,
            "industry_hotel": false,
            "industry_information_technology": false,
            "industry_aviation": false,
            "industry_mechanical_engineering": false,
            "industry_pharma": false,
            "industry_manufacturing": false,
            "industry_lawyer": false,
            "industry_other_services": false,
            "industry_telecommunications": false,
            "industry_associations": false,
            "industry_utility_provider": false,
            "industry_miscellaneous": false,
            "duration": "P1Y",
            "turnover": 499999900,
            "insured_sum": 10000000,
            "policy_start_date": "2019-04-17",
            "distributor_discount": "zurich"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

      {
        "token": "TOKEN",
        "gross_payment_amount": 65810,
        "extra_data": {
          "gross_premium": 65810,
          "premium_tax": 10510,
          "net_premium": 55300,
          "tax_rate": 0.19,
          "security_liability": 10000000,
          "breach_costs": 10000000,
          "business_interruption": 5000000,
          "regulatory_fines": 5000000,
          "pci": 5000000,
          "emergency_costs": 1000000,
          "cyber_terrorism": 5000000,
          "internet_media_liability": 5000000,
          "digital_asset_replacement": 5000000,
          "cyber_extortion": 2500000,
          "cyber_crime": 5000000,
          "hardware_damage": 2500000,
          "price": {
            "gross_premium": 65810,
            "net_premium": 55300,
            "premium_tax": 10510,
            "net_net_premium": 55300
          },
          "deductible": 200000,
          "risk_group": "normal_risk"
        }
      }

Create Unpaid Policy Request
----------------------------
JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20

   "company_name_with_legal_form", "string", Yes,   "Company Name",   "Kasko LTD"
   "company_website", "string", No,   "URL of company",   "https://www.kasko.io"
   "company_house_number", "string", Yes,   "House number of the companys address.",   "12"
   "company_street", "string", Yes,   "Street name of the company address.",   "Main street"
   "company_city", "string", Yes,   "City name of company.",  "Hamburg"
   "company_postcode", "string", Yes,   "Postcode of the company address.",   "10115"
   "company_country",  "string", Yes,   "Country of Company  (DE required at launch)",   "DE"
   "salutation", "string", Yes,   "Salutation",   "mr|mrs"
   "metadata", "json", No, "Optional metadata", ""


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
            "company_name_with_legal_form": "Kasko",
            "company_website": "www.kasko.io",
            "company_street": "123",
            "company_house_number": "123",
            "company_postcode": "10115",
            "company_city": "Hamburg",
            "company_country": "DE",
            "salutation": "mr",
            "phone": "+49711111"
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
   "provider",  "yes", "``string``", "Payment provider ``zurich_invoice``."
   "metadata.account_holder_name",  "yes", "``string``", "Account name``Kasko``."
   "metadata.iban",  "yes", "``string``", "Account IBAN``NO9386011117947``."
   "metadata.bic",  "yes", "``string``", "Account BIC ``12345678``."

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
            "method": "invoice",
            "provider": "zurich_invoice",
            "metadata": {
                  "account_holder_name": "Kasko",
                  "iban": "NO9386011117947",
                  "bic": "12345678"
            }
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
