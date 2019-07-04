REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint .**

V2 Header
----------

The API requests must use the V2 header as show in the examples below:

``Accept: application/vnd.kasko.v2+json``

Quote Data
----------

.. csv-table::
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20

    "company_non_insurable2", "boolean", Yes, "No legal disputes in connection with an activity as general contractor.", "true|false"
    "company_non_insurable3", "boolean", Yes, "No SME liability insurance was cancelled by an insurer for this company.", "true|false"
    "company_non_insurable4", "boolean", Yes, "This company has had less than three claims in the last three years or if more, then the costs amounted to max. 5000 swiss francs.", "true|false"
    "company_non_insurable5", "boolean", Yes, "This company has fought less than three court cases in the last three years.", "true|false"
    "industry_code", "string", No, "Company industry code.", "711101.00"
    "salary_sum", "integer", Yes, "Annual payroll sum.", "100000"
    "turnover", "integer", Yes, "Company annual turnover.", "100000"
    "cover_type", "string", Yes, "Selected cover (ECO, SMART).", "NULL|ECO|SMART"
    "rented_flat_count", "integer", No, "Number of insured residential units.", "4"
    "insured_household_count", "integer", No, "Number of insured households.", "4"
    "license_plate_count", "integer", No, "Number of insured licence plates.", "10"
    "driver_insurance", "boolean", No, "Business trips with private vehicles insured.", "true|false"
    "duration", "string", Yes, "Contract duration.", "P1Y|P3Y|P5Y"
    "payment_frequency", "string", Yes, "Payment frequency of the contract.", "annual|semi_annual|quarter"
    "policy_start_date", "string", Yes, "Desired start date of the contract ISO 8601 format.", "yyyy-mm-dd"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
      'https://api.kasko.io/quotes' \
      -H 'Accept: application/vnd.kasko.v2+json' \
      -H 'Content-Type: application/json' \
      -u <YOUR SECRET API KEY>: \
      -d '{
        "language": "de",
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID>",
        "subscription_plan_id": "<SUBSCRIPTION PLAN ID>",
        "data": {
            "company_non_insurable2": false,
            "company_non_insurable3": true,
            "company_non_insurable4": true,
            "company_non_insurable5": true,
            "industry_code": "711101.00",
            "salary_sum": 100000,
            "turnover": 100000,
            "cover_type": "ECO",
            "rented_flat_count": 4,
            "insured_household_count": 4,
            "license_plate_count": 10,
            "driver_insurance": true,
            "duration": "P1Y",
            "payment_frequency": "annual",
            "policy_start_date": "2019-06-12"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
      "token": "<QUOTE TOKEN>",
      "gross_payment_amount": 285460,
      "extra_data": {
        "gross_premium": 285460,
        "premium_tax": 13590,
        "net_premium": 271870,
        "tax_rate": 0.05,
        "insured_sum": {
          "base_price": 80000,
          "worldwide": 15000000,
          "switzerland": 100000000,
          "additional_contract_cover": 0,
          "competitive_cover": 0,
          "debt_collection_cover": 0,
          "all_rights": 0
        },
        "yearly_price": {
          "gross": 285460,
          "net": 271870,
          "tax": 13590
        },
        "half_yearly_price": {
          "gross": 142730,
          "tax": 7140,
          "net": 135590
        },
        "quarterly_price": {
          "gross": 71370,
          "tax": 3570,
          "net": 67800
        },
        "modules": {
          "rented_flat": 60630,
          "insured_household": 128650,
          "license_plate": 47770,
          "driver_insurance": 4860,
          "eco": 29960,
          "smart_diff": 0,
          "top_diff": 0
        },
        "policy_start_date": "2019-06-12",
        "policy_end_date": "2020-12-31 23:59:59"
      }
    }

Create Offer Request
----------------------------

.. csv-table::
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20



    "crefo_id",                 "string", No,  "Credit Reform id.", "40000000"
    "crefo_uid",                "string", No,  "Credit Reform unique id.", "CHE-000.000.000"
    "company_industry",         "string", Yes, "Company industry name.", "Architekturbüros"
    "company_legal_form",       "string", No,  "Company legal form: `10` - `Einzelfirma`, `15` - `Unbekannte Rechtsform`, `20` - `Einfache Gesellschaft`, `21` - `Kollektivgesellschaft`, `22` - `Kommanditgesellschaft`, `23` - `Treuhänderschaft/Treuunternehmen`, `29` - `Europäische Gesellschaft`, `30` - `Genossenschaft`, `31` - `Aktiengesellschaft`, `32` - `Kommandit Aktiengesellschaft`, `33` - `GmbH`, `34` - `Stiftung`, `36` -` Verein`, `38` - `Anstalt FL`, `40` - `Formloser Bericht`, `41` - `Zweigniederl. ausländ.Gesellsch.`, `50` - `Anstalt des öffentl. Rechts`, `55` - `Institut des öffentl. Rechts`, `60` - `Oeffentl. rechtl. Körperschaft`.", "10|15|20|21|22|23|29|30|31|32|33|34|36|38|40|41|50|55|60"
    "company_name",             "string", Yes, "Insured company's name.", "Kasko"
    "salutation",               "string", Yes, "Policyholder's title.", "mr|ms"
    "phone",                    "string", Yes, "Policyholder's phone number.", "+41730420000"
    "street",                   "string", Yes, "Street name of the policyholder's address.", "Main street"
    "postcode",                 "string", Yes, "Postcode of the policyholder's address.", "1234"
    "house_number",             "string", No,  "House number of the policyholder's address.", "12"
    "city",                     "string", Yes, "City of the policyholder's address.", "Basel"
    "po_box",                   "string", Yes, "Policyholder's mailbox", "12345"
    "household1_salutation",    "string", Yes, "Title of the first residence owner.", "mr|ms"
    "household1_first_name",    "string", Yes, "First name of the first residence owner.", "John1"
    "household1_last_name",     "string", Yes, "Last name of the first residence owner.", "Doe1"
    "household1_street",        "string", Yes, "Street name of the first residence owner's address.", "Main1 street"
    "household1_postcode",      "string", Yes, "Postcode of the first residence owner's address.", "1234"
    "household1_house_number",  "string", Yes, "House number of the first residence owner's address.", "11"
    "household1_city",          "string", Yes, "City of the first residence owner's address.", "Vessy"
    "household2_salutation",    "string", Yes, "Title of the second residence owner.", "mr|ms"
    "household2_first_name",    "string", Yes, "First name of the second residence owner.", "John2"
    "household2_last_name",     "string", Yes, "Last name of the second residence owner.", "Doe2"
    "household2_street",        "string", Yes, "Street name of the second residence owner's address.", "Main2 street"
    "household2_postcode",      "string", Yes, "Postcode of the second residence owner's address.", "2345"
    "household2_house_number",  "string", Yes, "House number of the second residence owner's address.", "22"
    "household2_city",          "string", Yes, "City of the second residence owner's address.", "Le Cerneux-Veusil"
    "household3_salutation",    "string", Yes, "Title of the third residence owner.", "mr|ms"
    "household3_first_name",    "string", Yes, "First name of the third residence owner.", "John3"
    "household3_last_name",     "string", Yes, "Last name of the third residence owner.", "Doe3"
    "household3_street",        "string", Yes, "Street name of the third residence owner's address.", "Main3 street"
    "household3_postcode",      "string", Yes, "Postcode of the third residence owner's address.", "3456"
    "household3_house_number",  "string", Yes, "House number of the third residence owner's address.", "33"
    "household3_city",          "string", Yes, "City of the third residence owner's address.", "Trachselwald"
    "household4_salutation",    "string", Yes, "Title of the fourth residence owner.", "mr|ms"
    "household4_first_name",    "string", Yes, "First name of the fourth residence owner.", "John4"
    "household4_last_name",     "string", Yes, "Last name of the fourth residence owner.", "Doe4"
    "household4_street",        "string", Yes, "Street name of the fourth residence owner's address.", "Main4 street"
    "household4_postcode",      "string", Yes, "Postcode of the fourth residence owner's address.", "4566"
    "household4_house_number",  "string", Yes, "House number of the fourth residence owner's address.", "44"
    "household4_city",          "string", Yes, "City of the fourth residence owner's address.", "Halten"
    "household5_salutation",    "string", Yes, "Title of the fifth residence owner.", "mr|ms"
    "household5_first_name",    "string", Yes, "First name of the fifth residence owner.", "John5"
    "household5_last_name",     "string", Yes, "Last name of the fifth residence owner.", "Doe5"
    "household5_street",        "string", Yes, "Street name of the fifth residence owner's address.", "Main5 street"
    "household5_postcode",      "string", Yes, "Postcode of the fifth residence owner's address.", "2560"
    "household5_house_number",  "string", Yes, "House number of the fifth residence owner's address.", "55"
    "household5_city",          "string", Yes, "City of the fifth residence owner's address.", "Nidau"
    "duration",                 "string", Yes, "Contract duration.", "P1Y|P3Y|P5Y"
    "policy_start_date",        "string", Yes, "Desired start date of the contract ISO 8601 format.", "yyyy-mm-dd"
    "policy_purchase_email",    "string", Yes, "To whom the policy will be sent.", "test+receiver@kasko.io"

    "payment_metadata.company_paying",  "boolean",  Yes, "Boolean to indicate if Payer is company or private person.", "true|false"
    "payment_metadata.company_name",    "string",   Yes, "Required if company_paying true. Payer's company name.", "Kasko"
    "payment_metadata.salutation",      "string",   Yes, "Required if company_paying false. Payer's salutation.", "mr|ms"
    "payment_metadata.first_name",      "string",   Yes, "Required if company_paying false. Payer's first name.", "John"
    "payment_metadata.last_name",       "string",   Yes, "Required if company_paying false. Payer's last name.", "Doe"
    "payment_metadata.house_number",    "string",   No,  "House number of the payer's address.", "11"
    "payment_metadata.street",          "string",   Yes, "Street name of the payer's address.", "Less Main street"
    "payment_metadata.postcode",        "string",   Yes, "Postcode of the payer's address.", "Zurich"
    "payment_metadata.po_box",          "string",   No,  "Payer's mailbox.", "12345"
    "payment_metadata.city",            "string",   Yes, "City of the payer's address.", "1989"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'Content-Type: application/json' \
        -u <YOUR SECRET API KEY>: \
        -d '{
          "data": {
           "crefo_id": "40000000",
            "crefo_uid": "CHE-000.000.000",
            "company_industry": "Architekturbüros",
            "company_legal_form": "10",
            "company_name": "Kasko",
            "salutation": "mr",
            "phone": "+41730420000",
            "street": "Bielstrasse",
            "postcode": "2560",
            "house_number": "1",
            "city": "Nidau",
            "po_box": "12345",
            "household1_salutation": "mr",
            "household1_first_name": "John1",
            "household1_last_name": "Doe1",
            "household1_street": "Main1 street",
            "household1_postcode": "1234",
            "household1_house_number": "111",
            "household1_city": "Vessy",
            "household2_salutation": "mr",
            "household2_first_name": "John2",
            "household2_last_name": "Doe2",
            "household2_street": "Main2 street",
            "household2_postcode": "2345",
            "household2_house_number": "222",
            "household2_city": "Le Cerneux-Veusil",
            "household3_salutation": "mr",
            "household3_first_name": "John3",
            "household3_last_name": "Doe3",
            "household3_street": "Main3 street",
            "household3_postcode": "3456",
            "household3_house_number": "333",
            "household3_city": "Trachselwald",
            "household4_salutation": "mr",
            "household4_first_name": "John4",
            "household4_last_name": "Doe4",
            "household4_street": "Main4 street",
            "household4_postcode": "4566",
            "household4_house_number": "444",
            "household4_city": "Halten",
            "duration": "P1Y",
            "policy_start_date": "2019-06-11",
            "policy_purchase_email": "test+receiver@kasko.io",
            "payment_metadata": {
                "company_paying": true,
                "company_name": "kasko",
                "house_number": "11",
                "street": "Less Main street",
                "postcode": "1989",
                "po_box": "12AS1Q"
                "city": "Zurich"
            }
          },
          "email": "test@kasko.io",
          "first_name": "First_name",
          "language": "de",
          "last_name": "Last_name",
          "quote_token": "<QUOTE TOKEN>"
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
      "id": "<POLICY ID>",
      "insurer_policy_id": "TEST-RS-XXXXXXXXXX",
      "insurer_external_policy_id": null,
      "payment_token": "<PAYMENT TOKEN>",
      "_links": {
        "_self": {
          "href": "https:\/\/api.kasko.io\/policies\/<POLICY ID>"
        }
      }
    }

.. _OfferResponse:

Convert offer to policy (payment)
---------------------------------

To create a policy you should convert offer to policy. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by OfferResponse_."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by OfferResponse_."

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "method": "distributor",
            "provider": "distributor",
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>"
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
