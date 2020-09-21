REST API - Dog Surgery
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

   "gross_premium", "integer", "Gross premium calculated by the distributor", "10000000"
   "policy_start_date", "string", "Policy start date in ISO date format", "2020-01-01"
   "policy_end_date", "string", "Policy end_date in ISO date format", "2020-02-01"

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
            "gross_premium": "100000",
            "policy_start_date": "2020-01-01",
            "policy_end_date": "2020-02-01"
        }
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "token": "<QUOTE_TOKEN",
        "gross_payment_amount": 100000,
        "extra_data": {
            "gross_premium": "100000",
            "premium_tax": 15966,
            "net_premium": 84034,
            "tax_rate": 0.19
        }
    }

Lead Request
^^^^^^^^^^^^
JSON data posted to /leads to create a lead

.. csv-table::
   :header: "Name", "Type", "Example Value"
   :widths: 20, 20, 20

   "touchpoint_id", "string", "<TOUCHPOINT_ID>"
   "item_id", "string", "<ITEM_ID>"
   "first_name", "string", "firstName"
   "last_name", "string", "lastName"
   "dob", "string", "1993-12-02"
   "policy_start_date", "string", "2020-01-01"
   "policy_end_date", "string", "2020-02-01"
   "dog_name", "string", "dogName"
   "dog_hybrid", "string", "dog hybrid"
   "dog_breed", "string", "doberman"
   "dog_dangerous", "string", "No"
   "registration", "string", "Yes"
   "coverage", "string", "Yes"
   "deductible", "string", "Yes"
   "payment_method", "string", "credit card"
   "payment_frequency", "string", "1"
   "annual_gross_premium", "string", "1000000"
   "email", "string", "test@kasko.io"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
          'https://api.kasko.io/leads' \
          -H 'Content-Type: application/json' \
          -u <SECRET_KEY>: \
          -d '{
            "item_id": "<ITEM_ID>",
            "touchpoint_id": "<TOUCHPOINT_ID>",
            "first_name": "firstName",
            "last_name": "lastName",
            "email": "test@kasko.io",
            "language": "de",
            "data": {
                "dob": "1993-12-02",
                "policy_start_date": "2020-01-01",
                "policy_end_date": "2020-02-01",
                "dog_name": "dogName",
                "dog_hybrid": "no",
                "dog_breed": "doberman",
                "dog_dangerous": "no",
                "registration": "yes",
                "coverage": "no",
                "deductible": "yes",
                "payment_method": "credit card",
                "payment_frequency": "monthly",
                "annual_gross_premium": "10000000"
            },
            "quote_token": "<QUOTE_TOKEN>"
        }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "id": "<LEAD_ID>",
        "livemode": false,
        "firstname": "firstName",
        "lastname": "lastName",
        "email": "test@kasko.io",
        "quote": {
            "version": 2,
            "customer_input": {
                "gross_premium": "100000",
                "policy_start_date": "2020-01-01",
                "policy_end_date": "2020-02-01"
            },
            "touchpoint_id": "<TOUCHPOINT_ID>",
            "item_id": "<ITEM_ID>",
            "subscription_plan_id": "<SUBSCRIPTION_ID>",
            "gross_payment_amount": 100000,
            "payment_data": {
                "gross_premium": 100000,
                "premium_tax": 15966,
                "net_premium": 84034,
                "net_net_premium": 62185,
                "net_commission_total": 21849,
                "tax_rate": 0.19
            },
            "data": [],
            "duration_strategy": "fixed_start_and_end_date",
            "duration_data": {
                "policy_start_date": "2019-12-31T23:00:00+00:00",
                "policy_end_date": "2020-01-31T23:00:00+00:00"
            },
            "billing_cycles": 1,
            "quote_created_at": "2020-09-17T09:29:02+00:00",
            "signature": "<SIGNATURE>"
        },
        "data": {
            "dob": "1993-12-02",
            "policy_start_date": "2020-01-01",
            "policy_end_date": "2020-02-01",
            "dog_name": "dogName",
            "dog_hybrid": "no",
            "dog_breed": "doberman",
            "dog_dangerous": "no",
            "registration": "yes",
            "coverage": "no",
            "deductible": "yes",
            "payment_method": "credit card",
            "payment_frequency": "monthly",
            "annual_gross_premium": "10000000",
            "access_pin": "VPE3TEFH"
        },
        "product_id": "<ITEM_ID>",
        "policy_id": null,
        "touchpoint_id": "<TOUCHPOINT_ID>",
        "distributor_id": "<DISTRIBUTOR_ID>",
        "referrer_url": null,
        "whitelisted_referrer_url": null,
        "metadata": null,
        "opt_out_date": null,
        "language": "de",
        "currency": "EUR",
        "updated_at": "2020-09-17T12:40:09+00:00",
        "created_at": "2020-09-17T12:40:09+00:00",
        "assets": [],
        "token": "<TOKEN>>",
        "status": "pending",
        "_links": {
            "_self": {
                "href": "https://api.kasko.io/leads/<LEAD_ID>"
            },
            "distributor": {
                "href": "https://api.kasko.io/accounts/<DISTRIBUTOR_ID>"
            },
            "product": {
                "href": "https://api.kasko.io/products/<ITEM_ID>"
            }
        }
    }

Show Lead
~~~~~~~~~

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

        curl -X GET https://api.kasko.io/leads/<LEAD_ID> \
        -u <SECRET_KEY>: \
        -H 'Content-Type: application/json'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "id": "<LEAD_ID>",
        "livemode": false,
        "firstname": "firstName",
        "lastname": "lastName",
        "email": "test@kasko.io",
        "quote": {
            "version": 2,
            "customer_input": {
                "gross_premium": "100000",
                "policy_start_date": "2020-01-01",
                "policy_end_date": "2020-02-01"
            },
            "touchpoint_id": "<TOUCHPOINT_ID>",
            "item_id": "<ITEM_ID>",
            "subscription_plan_id": "<SUBSCRIPTION_ID>",
            "gross_payment_amount": 100000,
            "payment_data": {
                "gross_premium": 100000,
                "premium_tax": 15966,
                "net_premium": 84034,
                "net_net_premium": 62185,
                "net_commission_total": 21849,
                "tax_rate": 0.19
            },
            "data": [],
            "duration_strategy": "fixed_start_and_end_date",
            "duration_data": {
                "policy_start_date": "2019-12-31T23:00:00+00:00",
                "policy_end_date": "2020-01-31T23:00:00+00:00"
            },
            "billing_cycles": 1,
            "quote_created_at": "2020-09-17T09:29:02+00:00",
            "signature": "<SIGNATURE>"
        },
        "data": {
            "dob": "1993-12-02",
            "policy_start_date": "2020-01-01",
            "policy_end_date": "2020-02-01",
            "dog_name": "dogName",
            "dog_hybrid": "no",
            "dog_breed": "doberman",
            "dog_dangerous": "no",
            "registration": "yes",
            "coverage": "no",
            "deductible": "yes",
            "payment_method": "credit card",
            "payment_frequency": "monthly",
            "annual_gross_premium": "10000000",
            "access_pin": "Y7Y9X7NT"
        },
        "product_id": "<ITEM_ID>",
        "policy_id": null,
        "touchpoint_id": "<TOUCHPOINT_ID>",
        "distributor_id": "<DISTRIBUTOR_ID>",
        "referrer_url": null,
        "whitelisted_referrer_url": null,
        "metadata": null,
        "opt_out_date": null,
        "language": "de",
        "currency": "EUR",
        "updated_at": "2020-09-17T14:41:10+00:00",
        "created_at": "2020-09-17T14:41:08+00:00",
        "assets": [
            {
                "name": "Vorschlag",
                "extension": "pdf",
                "url": "https://asset-url.com",
                "designation": "invoice",
                "attachment_date": "2020-09-17T14:41:10.000000Z"
            }
        ],
        "token": "<TOKEN>",
        "status": "ready",
        "_links": {
            "_self": {
                "href": "https://api.kasko.io/leads/toG9v1ZzQABWJxbonZAagwOL0EMrNYp3n"
            },
            "distributor": {
                "href": "https://api.kasko.io/accounts/<DISTRIBUTOR_ID>"
            },
            "product": {
                "href": "https://api.kasko.io/products/<ITEM_ID>"
            }
        }
    }

Create Unpaid Policy Request
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

JSON data posted to /policies on creation of unpaid policy.

.. csv-table::
   :header: "Name", "Type", "Example Value"
   :widths: 20, 20, 20

   "first_name", "string", "firstName"
   "last_name", "string", "lastName"
   "salutation", "string", "ms"
   "address_supplement", "string", "addressSupplement"
   "street", "string", "test st."
   "house_number", "string", "42"
   "postcode", "string", "12345"
   "nationality", "string", "Latvian"
   "phone_number", "string", "+999 233445566"
   "email", "string", "kasko@kasko.io"
   "martial_status", "string", "single"
   "different_contributor", "boolean", "true"
   "iban", "string", "DE89370400440532013000"
   "sepa_issued_date", "string", "2020-01-01"
   "payment_frequency", "string", "monthly"
   "payment_method", "string", "invoice"
   "tax_office", "boolean", true
   "new_application", "boolean", true
   "replacement_policy", "boolean", false
   "old_policy_number", "string", "oldPolicyNumber"
   "policy_start_date", "string", "2020-02-01"
   "policy_end_date", "string", "2020-03-01"
   "insured_dog", "boolean", "true"
   "insured_cat", "boolean", "true"
   "deductible", "string", "250"
   "insured_module", "string", "best"
   "dental_module", "boolean", "true"
   "pet_name", "string", "petName"
   "pet_gender", "string", "Male"
   "pet_dob", "string", "2001-01-01"
   "pet_breed", "string", "doberman"
   "mixed_breed", "boolean", "true"
   "dog_shoulder_height", "string", "above_45"
   "cat_type", "string", "home"
   "pet_id", "string", "tattoo"
   "pet_id_number", "string", "123123123"
   "net_premium", "string", "10000"
   "operation_in_3_years", "boolean", "true"
   "type_of_operation_1", "string", "operationType1"
   "vet_name_1", "string", "vetName1"
   "operation_date_1", "string", "2020-03-01"
   "type_of_operation_2", "string", "operationType2"
   "vet_name_2", "string", "vetName2"
   "operation_date_2", "string", "2020-03-05"
   "type_of_operation_3", "string", "operationType3"
   "vet_name_3", "string", "vetName3"
   "operation_date_3", "string", "2020-03-10"
   "previous_insurance", "boolean", "true"
   "previous_insurance_detail", "string", "dog_surgery"
   "previous_insurer_1", "string", "previousInsurer1"
   "previous_insurance_number_1", "string", "insuranceNumber1"
   "previous_insurance_type_1", "string", "insuranceType1"
   "previous_insurance_terminator_1", "string", "insuranceTerminator1"
   "previous_insurer_2", "string", "previousInsurer2"
   "previous_insurance_number_2", "string", "insuranceNumber2"
   "previous_insurance_type_2", "string", "insuranceType2"
   "previous_insurance_terminator_2", "string", "insuranceTerminator2"
   "previous_insurer_3", "string", "previousInsurer3"
   "previous_insurance_number_3", "string", "insuranceNumber3"
   "previous_insurance_type_3", "string", "insuranceType3"
   "previous_insurance_terminator_3", "string", "insuranceTerminator3"
   "special_agreement", "string", "specialAgreement"
   "cat_description", "string", "cute"
   "information_channel", "string", "email"
   "sign_place", "string", "signPlace"
   "sign_date", "string", "2020-03-01"
   "agent_name", "string", "agentName"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl -X POST \
        'https://api.kasko.io/policies' \
        -H 'Accept: application/vnd.kasko.v2+json' \
        -H 'Content-Type: application/json' \
        -u <SECRET_KEY>: \
        -d '{
            "data": {
               "salutation": "ms",
               "address_supplement": "addressSupplement",
               "street": "test st.",
               "house_number": "42",
               "postcode": "12345",
               "nationality": "Latvian",
               "phone_number": "+999 233445566",
               "martial_status": "single",
               "different_contributor": "true",
               "iban": "DE89370400440532013000",
               "sepa_issued_date": "2020-01-01",
               "payment_frequency": "monthly",
               "payment_method": "invoice",
               "tax_office": "true",
               "new_application": "true",
               "replacement_policy": "false",
               "old_policy_number": "oldPolicyNumber",
               "policy_start_date": "2020-01-01",
               "policy_end_date": "2020-03-01",
               "insured_dog": "true",
               "insured_cat": "true",
               "deductible": "250",
               "insured_module": "best",
               "dental_module": "true",
               "pet_name": "petName",
               "pet_gender": "male",
               "pet_dob": "2001-01-01",
               "pet_breed": "doberman",
               "mixed_breed": "true",
               "dog_shoulder_height": "above_45",
               "cat_type": "home",
               "pet_id": "tattoo",
               "pet_id_number": "123123123",
               "net_premium": "10000",
               "operation_in_3_years": true,
               "type_of_operation_1": "operationType1",
               "vet_name_1": "vetName1",
               "operation_date_1": "2020-03-01",
               "type_of_operation_2": "operationType2",
               "vet_name_2": "vetName2",
               "operation_date_2": "2020-03-05",
               "type_of_operation_3": "operationType3",
               "vet_name_3": "vetName3",
               "operation_date_3": "2020-03-10",
               "previous_insurance": true,
               "previous_insurance_detail": "prevInsuranceDetail",
               "previous_insurer_1": "prevInsurer",
               "previous_insurance_number_1": "prevInsuranceNumber",
               "previous_insurance_type_1": "prevInsuranceType",
               "previous_insurance_terminator_1": "prevInsuranceTerminator",
               "previous_insurer_2": "prevInsurer2",
               "previous_insurance_number_2": "prevInsuranceNumber2",
               "previous_insurance_type_2": "prevInsuranceType2",
               "previous_insurance_terminator_2": "prevInsuranceTerminator2",
               "previous_insurer_3": "prevInsurer3",
               "previous_insurance_number_3": "prevInsuranceNumber3",
               "previous_insurance_type_3": "prevInsuranceType3",
               "previous_insurance_terminator_3": "prevInsuranceTerminator3",
               "special_agreement": "No",
               "cat_description": "cute",
               "information_channel": "email",
               "sign_place": "signPlace",
               "sign_date": "2020-03-01",
               "agent_name": "agentName"
            },
            "email": "test@kasko.io",
            "first_name": "First name",
            "language": "de",
            "last_name": "Last name",
            "quote_token": "<TOKEN>"
    }'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "id": "<POLICY_ID>",
        "insurer_policy_id": "<INSURER_POLICY_ID>",
        "payment_token": "<PAYMENT_TOKEN>",
        "_links": {
            "_self": {
                "href": "https:\/\/api.kasko.io\/policies\/<POLICY_ID>"
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

Show Policy
~~~~~~~~~~~

Example Request
~~~~~~~~~~~~~~~

.. code-block:: bash

    curl -X GET https://api.kasko.io/policies/<POLICY_ID> \
    -u <SECRET_KEY>: \
    -H 'Accept: application/vnd.kasko.v2+json' \
    -H 'Content-Type: application/json'

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

    {
        "id": "<POLICY_ID>",
        "version_id": "<POLICY_VERSION_ID>",
        "distributor_id": "<DISTRIBUTOR_ID>",
        "insurer_id": "<INSURER_ID>",
        "touchpoint_id": "<TOUCHPOINT_ID>",
        "integration_version_id": null,
        "item_id": "<ITEM_ID>",
        "customer_id": "<CUSTOMER_ID>",
        "subscription_plan_id": "<SUBSCRIPTION_PLAN_ID>",
        "insurer_policy_id": "<INSURER_POLICY_ID>",
        "insurer_external_policy_id": null,
        "linked_policy_id": null,
        "expired": false,
        "first_name": "First name",
        "last_name": "Last name",
        "email": "test@kasko.io",
        "currency": "eur",
        "policy_created_date": "2020-09-20T10:21:10+00:00",
        "start_date": "2019-12-31T23:00:00+00:00",
        "end_date": "2020-01-31T23:00:00+00:00",
        "termination_date": null,
        "language": "de",
        "status": "paid",
        "data": {
            "payment_metadata": []
        },
        "quote": {
            "version": 2,
            "customer_input": {
                "gross_premium": "100000",
                "policy_start_date": "2020-01-01",
                "policy_end_date": "2020-02-01"
            },
            "touchpoint_id": "<TOUCHPOINT_ID>",
            "item_id": "<ITEM_ID>",
            "subscription_plan_id": "<SUBSCRIPTION_PLAN_ID>",
            "gross_payment_amount": 100000,
            "payment_data": {
                "gross_premium": 100000,
                "premium_tax": 15966,
                "net_premium": 84034,
                "net_net_premium": 62185,
                "net_commission_total": 21849,
                "tax_rate": 0.19
            },
            "data": [],
            "duration_strategy": "fixed_start_and_end_date",
            "duration_data": {
                "policy_start_date": "2019-12-31T23:00:00+00:00",
                "policy_end_date": "2020-01-31T23:00:00+00:00"
            },
            "billing_cycles": 1,
            "quote_created_at": "2020-09-20T10:16:11+00:00"
        },
        "metadata": {},
        "cancellation_reason": null,
        "cancelled_by": "",
        "cancelled_at": null,
        "distributor_traffic_source": null,
        "referrer_url": null,
        "whitelisted_referrer_url": null,
        "deleted_at": null,
        "deleted_by": "",
        "assets": [],
        "media": [],
        "is_locked": false,
        "localized_dates": {
            "start_date": "2020-01-01T00:00:00+01:00",
            "end_date": "2020-02-01T00:00:00+01:00",
            "termination_date": null,
            "policy_created_date": "2020-09-20T12:21:10+02:00",
            "time_zone": "Europe\/Berlin"
        },
        "_links": {
            "_self": {
                "href": "https:\/\/api.kasko.io\/policies\/<POLICY_ID>"
            },
            "distributor": {
                "href": "https:\/\/api.kasko.io\/accounts\/<DISTRIBUTOR_ID>"
            },
            "insurer": {
                "href": "https:\/\/api.kasko.io\/accounts\/<INSURER_ID>"
            },
            "item": {
                "href": "https:\/\/api.kasko.io\/items\/<ITEM_ID>"
            },
            "touchpoint": {
                "href": "https:\/\/api.kasko.io\/touchpoints\/<TOUCHPOINT_ID>"
            }
        }
    }
