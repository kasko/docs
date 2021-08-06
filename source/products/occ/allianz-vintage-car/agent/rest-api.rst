========
REST API (AGENT)
========

.. note::  Refer to :ref:`REST API page<rest_api>` for more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint.**

Headers
=======

1. ``Accept: application/vnd.kasko.v2+json`` - V2 header. The API requests must include the V2 header in `Quote request`_, `Offer`_ requests.

Possible requests
=================

To purchase a policy at least 3 requests are required in the following order:

1. `Quote`_  - get the policy price.
2. `Offer`_ - create an offer.
3. `Payment`_ convert offer to a paid policy.

.. _Quote:

Quote request
-------------
This request calculates how much policyholder should pay for the policy.
Following factors are considered while calculating policy price:

.. csv-table::
   :header: "Name", "Type", "Required", "Example Value"
   :widths: 20, 20, 80, 20

   "vehicle_type",         "string", "YES", "car"
   "built_year",           "string", "YES", "1890"
   "insured_sum",          "integer","YES",  "150000"
   "coverage_type",        "string", "YES", "half"
   "deductible_a1",        "string", "YES if:coverage_type = half", "15000"
   "deductible_b1",        "string", "YES if:coverage_type = full", "30000"
   "deductible_b2",        "string", "YES if:coverage_type = full", "50000"
   "deductible_c1",        "string", "YES if:coverage_type = full_plus", "50000"
   "deductible_c2",        "string", "YES if:coverage_type = full_plus", "50000"
   "garage",               "string", "YES", "underground"
   "usage",                "string", "YES", "3000km"
   "car_type",             "string", "YES",  "dai_0a585zcVzbe9YhdmPzAxE82Kri3z"
   "max_horse_power",      "bool",   "YES", "true"
   "recovery_cost",        "bool",   "YES", "true"
   "daily_usage",          "bool",   "YES", "true"
   "daily_usage_car",      "bool",   "YES", "true"
   "driving_license_year", "string", "YES", "1996"
   "dob",                  "string", "YES", "1985-01-01"
   "young_driver",         "bool",   "YES", "true"
   "damages",              "bool",   "YES", "true"
   "lost_license",         "bool",   "YES", "true"
   "first_registration",   "string", "YES", "1995-01-01"
   "car_tuning",           "string", "YES", "true"
   "optin1",               "string", "NO",  "true"
   "payment_frequency",    "string", "YES", "yearly"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -H 'Accept: application/vnd.kasko.v2+json' \
       -d touchpoint_id=tp_9156ad4721199c4d309e3ba8a0e03 \
       -d item_id=item_e8d48fad65c9ca2f1dd5cd611a4 \
       -d subscription_plan_id=sp_4776780b64fd0a427e61b3a764123 \
       -d data='{"built_year":"1998","car_tuning":true,"car_type":"dai_1D80gxtO3RMO8qyxx1R5at5MLPck","coverage_type":"full","daily_usage":false,"daily_usage_car":true,"damages":false,"deductible_a1":15000,"deductible_b1":50000,"deductible_b2":50000,"deductible_c1":50000,"deductible_c2":15000,"dob":"1985-01-01","driving_license_year":"2000","first_registration":"1998-01-01","garage":"single","insured_sum":1900000,"lost_license":false,"max_horse_power":false,"payment_frequency":"yearly","policy_start_date":"2021-08-05","policy_validity_interval":"P1Y","recovery_cost":true,"usage":"5000km","vehicle_type":"car","young_driver":false,"optin1":true}'

Example response
~~~~~~~~~~~~~~~~
.. _QuoteResponse:

.. code:: bash

    {
      "token": "<QUOTE TOKEN>",
      "gross_payment_amount": 33300,
      "extra_data": {
        "gross_premium": 33300,
        "premium_tax": 3300,
        "net_premium": 30000,
        "tax_rate": 0.11,
        "minimum_value": 1900000,
        "maximum_value": 4900000,
        "suggested_sum": 3150000,
        "flow": "manual_underwriting",
        "car_category": "youngtimer",
        "car_coverage": "full",
        "mu_trigger": {
          "built_year": false,
          "insured_sum": false,
          "max_horse_power": false,
          "recovery_cost": true,
          "daily_usage_car": false,
          "driving_license_year": false,
          "young_driver": false,
          "car_condition_2": false,
          "car_tuning": false,
          "optin1": false,
          "heavy_truck": false,
          "body": false,
          "power_hp": false,
          "dob": false,
          "vehicle_negative_list": false,
          "condition_2_3_empty": false,
          "premium_car": false
        },
        "flow_soft_ko": true,
        "frequency_gross_premium": 33300,
        "frequency_premium_tax": 3300,
        "frequency_net_premium": 30000,
        "pro_rata": 12000
      }
    }


Create an offer (unpaid policy)
-------------------------------
.. _Offer:

This request stores policy holder information that is related to offer. Following information can be stored in offer:

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

    "account_owner", "string", "Phone number.", "+44222222222"
    "bank_name", "string", "Bank name.", "Test"
    "car_body_list", "array", "Car body list", {"0":"Cabriolet 2-Sitze","1":"Landaulet"}
    "car_id", "string", "Required if:new_client = false.", "test"
    "car_tariff_list", "array", "Car tariff list", {"0":"PKW offen","1":"PKW geschlossen","2":"LKW","3":"Wohnmobile","4":"Bus"}
    "city", "string", "City.", "dai_Q9bJSeYxIuhv1Vo903cCLPb4pIE0"
    "condition_2_min", "integer", "Condition 2 min", 0
    "condition_3_min", "integer", "Condition 2 min", 0
    "flag_purchase_lead", "bool", "Purhase lead flag", true
    "horse_power", "string", "Horse power.", "1234"
    "house_number", "string", "House number.", "1234"
    "iban", "string", "Iban", "GB29NWBK60161331926819"
    "insured_before", "string", "Insured before", true
    "license_plate_type", "string", "License plate type.", "shared"
    "main_driver", "bool", "Main driver", true
    "main_driver_title", "string", "Main driver title", "Test"
    "maker", "string", "Maker.", "1234"
    "maker_model", "string", "Maker model.", "1234"
    "miles", "string", "Miles or km", "km"
    "miles_value", "string", "Miles value.", "1234"
    "motorcycle_body_list", "array", "Motorcycle body list", {"0":"Kraftrad","1":"Schlepper","2":"Zugmaschine","3":"Roller","4":"Traktor","5":"Gespann"}
    "motorcycle_tariff_list", "array", "Motorcylce tariff list", {"0":"Traktor","1":"Krad","2":"Anhänger"}
    "newsletter_optin", "bool", "Agree of newsletter.", "true"
    "offer_recipient", "string", "Offer recipient", "test@test.lv"
    "offers_recipient", "string", "Offer recipient", "test@test.lv"
    "payment_method", "string", "Payment method", "invoice"
    "phone", "string", "Phone number", "+43222222222"
    "postcode", "string", "Postcode", "1130"
    "purchase_lead", "bool", "Purchase lead", true
    "salutation", "string", "Salutation", "mr"
    "street", "string", "Street", "Street"
    "title", "string", "Title", "dr"


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
                "account_owner": "Max Mustermann",
                "agent_city": "dai_xLIA1Wd5nqgt9jM7wi498Peq5BpR",
                "agent_email": "test@kasko.io",
                "agent_first_name": "Tewt",
                "agent_house_number": "Street 1",
                "agent_id": "2245356",
                "agent_last_name": "Test",
                "agent_mobile_number": "+4322222222",
                "agent_postcode": "1020",
                "agent_salutation": "ms",
                "agent_street": "Street",
                "bank_name": "Test",
                "car_body_list": {
                  "0": "Cabriolet 2-Sitze",
                  "1": "Landaulet",
                  "2": "Cabriolet 2-türig",
                  "3": "Lieferwagen",
                  "4": "Cabriolet 4-Sitze",
                  "5": "Limousine 2-türig",
                  "6": "Cabriolet 4-türig",
                  "7": "Limousine 3-türig",
                  "8": "Cabriolet",
                  "9": "Limousine 4-türig",
                  "10": "Cabriolimousine",
                  "11": "Limousine 5-türig",
                  "12": "Coupé (2+2)",
                  "13": "Limousine 6-türig",
                  "14": "Coupé 2-türig",
                  "15": "Limousine",
                  "16": "Coupé 3-türig",
                  "17": "Mini Bus",
                  "18": "Coupé 4-türig",
                  "19": "Pickup",
                  "20": "Coupé",
                  "21": "Pritsche-Doka",
                  "22": "Doppelkabine",
                  "23": "Pritsche",
                  "24": "Dreirad",
                  "25": "Pullman",
                  "26": "Fließheck-Lim. 2-türig",
                  "27": "Pullmann-Cabrio",
                  "28": "Fließheck-Lim. 4-türig",
                  "29": "Roadster",
                  "30": "Geländewagen",
                  "31": "Runabout",
                  "32": "Hardtop-Cabriolet",
                  "33": "Schrägheck-Lim. 2-türig",
                  "34": "Hardtop-Coupé",
                  "35": "Sport-Cabrio",
                  "36": "Hardtop-Lim. 2-türig",
                  "37": "Stretch-Limousine",
                  "38": "Hardtop-Lim. 4-türig",
                  "39": "Targa",
                  "40": "Kabinenroller",
                  "41": "Tourer",
                  "42": "Kastenwagen",
                  "43": "Traktor",
                  "44": "Kleinwagen",
                  "45": "Transporter",
                  "46": "Kombi (kurz)",
                  "47": "Wohnmobil",
                  "48": "Kombi (lang)",
                  "49": "Kombi 9 Sitzer",
                  "50": "Kombi 2-türig",
                  "51": "Kombi-Cpé. 3-türig",
                  "52": "Kombi 3-türig",
                  "53": "Kombi-Cpé. 5-türig",
                  "54": "Kombi 4-türig",
                  "55": "Kombi",
                  "56": "Kombi 5-türig",
                  "57": "Buggy"
                },
                "car_id": "555",
                "car_tariff_list": {
                  "0": "PKW offen",
                  "1": "PKW geschlossen",
                  "2": "LKW",
                  "3": "Wohnmobile",
                  "4": "Bus"
                },
                "city": "dai_wI2BVyYQ7Cq5qkiMAhu4bROIw6JH",
                "condition_2_min": 0,
                "condition_3_min": 0,
                "flag_purchase_lead": false,
                "horse_power": "150",
                "house_number": "22",
                "iban": "GB29NWBK60161331926819",
                "insured_before": true,
                "license_plate_type": "historic_license_plate",
                "main_driver": true,
                "main_driver_title": "ohne",
                "maker": "Alfa Romeo",
                "maker_model": "GTV 2.0 Twin Spark 16V (916)",
                "miles": "km",
                "miles_value": "50000",
                "motorcycle_body_list": {
                  "0": "Kraftrad",
                  "1": "Schlepper",
                  "2": "Zugmaschine",
                  "3": "Roller",
                  "4": "Traktor",
                  "5": "Gespann"
                },
                "motorcycle_tariff_list": {
                  "0": "Traktor",
                  "1": "Krad",
                  "2": "Anhänger"
                },
                "newsletter_optin": false,
                "offer_recipient": "test@kasko.io",
                "offers_recipient": "test@kasko.io",
                "payment_method": "invoice",
                "phone": "+4322222222",
                "postcode": "1200",
                "purchase_lead": false,
                "salutation": "ms",
                "street": "Street",
                "title": "ohne"
            },
            "quote_token":"<QUOTE TOKEN>",
            "first_name": "Test",
            "last_name": "Person",
            "email": "test@kasko.io",
            "language": "de"
    }'

NOTE. You should use ``<QUOTE TOKEN>`` value from `QuoteResponse`_.

Example response
~~~~~~~~~~~~~~~~
.. _OfferResponse:

.. code:: bash

    {
        "id": "<POLICY ID>",
        "insurer_policy_id": "<INSURER_POLICY_ID>",
        "payment_token": "<PAYMENT TOKEN>",
        "_links": {
            "_self": {
                "href": "https://api.kasko.io/policies/<POLICY ID>"
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

   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by `OfferResponse`_."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by `OfferResponse`_."
   "method",    "yes", "``string``", "Payment method ``invoice``."
   "provider",  "yes", "``string``", "Payment provider ``allianz_invoice``."

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u sk_test_SECRET_KEY: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT_TOKEN>",
            "policy_id": "<POLICY ID>",
            "method": "invoice",
            "provider": "allianz_invoice"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from `OfferResponse`_. After payment is made, policy creation is asynchronous.
