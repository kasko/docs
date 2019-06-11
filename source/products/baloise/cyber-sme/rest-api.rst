REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

**Please remember to use the API specific item and touchpoint .**

Quote Data
----------

.. csv-table::
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20


    "contract_type",                        "string",   Yes, "Insurance contract type.", "packet|maxi"
    "company_industry",                     "string",   Yes, "Company industry name.", "Architekturbüros"
    "company_industry_code",                "string",   No,  "Company industry code.", "711101.00"
    "company_employees",                    "integer",  Yes, "Number of employees working in the company.", "100"
    "company_turnover",                     "integer",  Yes, "Company's yearly turnover.", "123400"
    "company_percentage_webshop",           "string",   Yes, "Proportion of annual turnover achieved via online services or via webshop.", "0_10|11_25|26_50|51_75|76_100"
    "pci_optional_module",                  "boolean",  No,  "Company stores credit card information.", "true|false"
    "company_backup",                       "string",   Yes, "How often backups are made.", "no|irregular|monthly|weekly|daily"
    "company_patch",                        "string",   Yes, "How often software is updated.", "never|quarterly|monthly|immediately"
    "company_impact1",                      "string",   Yes, "Estimate after which the failure of the IT system business is impacted.", "from_0_12|from_12_72|from_72_more"
    "company_impact2",                      "string",   Yes, "Estimate how much IT failure will affect the business.", "low|medium|high|very_high"
    "package_selected",                     "string",   Yes, "Selected cover (ECO, SMART, TOP).", "S,M,L"
    "company_legal_form",                   "string",   No,  "Company legal form: `10` - `Einzelfirma`, `15` - `Unbekannte Rechtsform`, `20` - `Einfache Gesellschaft`, `21` - `Kollektivgesellschaft`, `22` - `Kommanditgesellschaft`, `23` - `Treuhänderschaft/Treuunternehmen`, `29` - `Europäische Gesellschaft`, `30` - `Genossenschaft`, `31` - `Aktiengesellschaft`, `32` - `Kommandit Aktiengesellschaft`, `33` - `GmbH`, `34` - `Stiftung`, `36` -` Verein`, `38` - `Anstalt FL`, `40` - `Formloser Bericht`, `41` - `Zweigniederl. ausländ.Gesellsch.`, `50` - `Anstalt des öffentl. Rechts`, `55` - `Institut des öffentl. Rechts`, `60` - `Oeffentl. rechtl. Körperschaft`.", "10|15|20|21|22|23|29|30|31|32|33|34|36|38|40|41|50|55|60"
    "deductible_package",                   "integer",  Yes, "Deductible.", "50000|100000|200000|500000|1000000"
    "deductible_time",                      "integer",  No,  "Waiting period Operating income loss.", "6|8|12|18|24|48"
    "insured_sum_own_damage",               "integer",  Yes, "Own damage insurance.", "5000000|10000000|25000000|50000000"
    "insured_sum_damage_to_3rd_parties",    "integer",  Yes, "Third party damage insurance.", "50000000|100000000|300000000|500000000"
    "insured_sum_assistance",               "integer",  No,  "Assistance insurance.", "5000000|10000000|25000000|50000000"
    "insured_sum_missed_income",            "integer",  No,  "Operating income loss insurance.", "0|5000000|10000000|25000000"
    "insured_sum_pci",                      "integer",  No,  "PCI compliance insurance.", "0|2000000"
    "insured_sum_ransom",                   "integer",  No,  "Ransom insurance.", "0|2000000"
    "contract_start_date",                  "string",   Yes, "Desired start date of the contract ISO 8601 format.", "2019-06-10"
    "contract_duration",                    "integer",  Yes, "Contract period.", "1|3|5"
    "contract_payment_frequency",           "string",   Yes, "Payment frequency of the contract.", "yearly|half-yearly|quarterly"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
      'https://api.kasko.io/quotes' \
      -H 'Content-Type: application/json' \
      -u <SECRET API KEY>: \
      -d '{
        "language": "de",
        "item_id": "<ITEM ID>",
        "touchpoint_id": "<TOUCHPOINT ID",
        "data": {
          "contract_type": "packet",
          "company_industry": "Architekturbüros",
          "company_industry_code": "711101.00",
          "company_employees": 100,
          "company_turnover": 123400,
          "company_percentage_webshop": "11_25",
          "company_non_insurable": [],
          "pci_optional_module": "",
          "company_backup": "weekly",
          "company_patch": "monthly",
          "company_impact1": "from_12_72",
          "company_impact2": "medium",
          "package_selected": "L",
          "company_legal_form": "10",
          "deductible_package": 100000,
          "deductible_time": 12,
          "insured_sum_own_damage": 5000000,
          "insured_sum_damage_to_3rd_parties": 50000000,
          "insured_sum_assistance": 5000000,
          "insured_sum_missed_income": 5000000,
          "insured_sum_pci": 0,
          "insured_sum_ransom": 2000000,
          "contract_start_date": "2019-06-10",
          "contract_duration": 1,
          "contract_payment_frequency": "yearly"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
      "signature": "<QUOTE TOKEN>",
      "gross_premium": 71950,
      "currency": "chf",
      "net_premium": 68520,
      "net_service_fee_total": 0,
      "premium_tax": 3430,
      "service_charge_vat": 0,
      "tax_rate": 0.05,
      "extra_data": {
        "risk_1": 3764.2148029497143,
        "risk_2": 2195.7919683873333,
        "risk_3": 1173.8510522825368,
        "risk_4": 506.9297507264585,
        "risk_5": 17928.64142188261,
        "risk_6": 23853.550375566207,
        "risk_7": 0,
        "risk_8": 1186.4247397445217,
        "risk_9": 593.2123698722609,
        "risk_10": 593.2123698722609,
        "risk_11": 593.2123698722609,
        "risk_12": 16135.376460525495
      }
    }

Create Unpaid Policy Request
----------------------------

.. csv-table::
   :header: "Name", "Type", "Required", "Description", "Example Value"
   :widths: 20, 20, 20, 80, 20

    "crefo_id",     "string", No,   "Credit Reform id.", "40000000"
    "company_uid",  "string", No,   "Company identifier.", "CH07310000000"
    "company_name", "string", Yes,  "Insured company's name.", "Kasko"
    "house_number", "string", No,   "House number of the policyholder's address.", "12"
    "street",       "string", Yes,  "Street name of the policyholder's address.", "Main street"
    "city",         "string", Yes,  "City of the policyholder's address.", "Basel"
    "postcode",     "string", Yes,  "Postcode of the policyholder's address.", "1234"
    "phone",        "string", Yes,  "Policyholder's phone number.", "+417304200"
    "salutation",   "string", Yes,  "Policyholder's title.", "mr|ms"

    "payment_metadata.company_paying",  "boolean",  Yes, "Boolean to indicate if Payer is company or private person.", "true|false"
    "payment_metadata.company_name",    "string",   Yes, "Required if company_paying true. Payer's company name.", "Kasko"
    "payment_metadata.salutation",      "string",   Yes, "Required if company_paying false. Payer's salutation.", "mr|ms"
    "payment_metadata.first_name",      "string",   Yes, "Required if company_paying false. Payer's first name.", "John"
    "payment_metadata.last_name",       "string",   Yes, "Required if company_paying false. Payer's last name.", "Doe"
    "payment_metadata.house_number",    "string",   No,  "House number of the payer's address.", "11"
    "payment_metadata.street",          "string",   Yes, "Street name of the payer's address.", "Less Main street"
    "payment_metadata.postcode",        "string",   Yes, "Postcode of the payer's address.", "Zurich"
    "payment_metadata.city",            "string",   Yes, "City of the payer's address.", "1989"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Content-Type: application/json' \
        -u <SECRET API KEY>: \
        -d '{
          "data": {
            "crefo_id": "40000000",
            "company_uid": "CH07310000000",
            "company_name": "Kasko",
            "house_number": "12",
            "street": "Main street",
            "city": "Basel",
            "postcode": "1234",
            "phone": "+417304200",
            "salutation": "mr",
            "payment_metadata": {
                "company_paying": true,
                "company_name": "kasko",
                "house_number": "11",
                "street": "Less Main street",
                "postcode": "1989",
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

.. code:: bash

    {
      "id": "<POLICY ID>",
      "insurer_policy_id": "TEST-CYBERSME-XXXXX",
      "insurer_external_policy_id": null,
      "payment_token": "<PAYMENT TOKEN>",
      "_links": {
        "_self": {
          "href": "https:\/\/api.kasko.io\/policies\/<POLICY ID>"
        }
      }
    }

.. _PolicyResponse:

Convert unpaid policy to paid (payment)
---------------------------------------

To create a policy you should convert unpaid policy to paid. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``<PAYMENT TOKEN>`` returned by PolicyResponse_ ."
   "policy_id", "yes", "``string``", "The 33 character long ``<POLICY ID>`` returned by PolicyResponse_."

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>"
        }'
