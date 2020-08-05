REST API
========

.. note::  Refer to :ref:`REST API page<rest_api>` for a more complete documentation regarding the necessary requests before performing and building your own.

Please remember to use the API specific ``ITEM_ID`` and ``TOUCHPOINT_ID``.

Possible requests
=================

At least 3 requests required in following order to buy policy:

1. Quote_ requests - specify required coverage & get an estimate of policy costs.
2. Offer_ requests - create an offer.
3. Payment_ requests - covert offer to policy.

Optional requests:

4. Receipt_ upload - upload receipt pdf.

.. _Quote:

Quote Data
----------
Quote data determines insurance coverages as well as affects price estimate. The query string data appended to the quote request.

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "duration",                              "string",   "Insurance duration. Available values (depending on item): P1Y, P2Y, P3Y, P4Y, P5Y", "P1Y"
   "item_value",                            "int",   "Item value in cents.", "200000"
   "older_than_three_months",               "bool",  "Has the item been purchased in the last 3 months?", "false"
   "theft",                                 "bool",  "Theft module.", "true"
   "damage",                                "bool",  "Damage module.", "false"
   "warranty",                              "bool",  "Warranty module (not available for all items).", "false"
   "loss",                                  "bool",  "Loss module (can be set to `true` only if theft is also `true`).", "false"
   "liability",                             "bool",  "Liability module (available only for items from Group 3).", "false"
   "item_purchase_date",                    "string",   "Item purchase date. Required if `warranty` module is enabled.", "2017-10-04"
   "item_manufacturers_warranty_duration",  "string",   "Item manufacturers warranty duration. Required if `warranty` module is enabled. Available values: P1Y, P2Y ... P9Y, P10Y.", "P1Y"


Quote Request
~~~~~~~~~~~~~~~
Sample quote request:

.. code:: bash

   curl --get https://api.kasko.io/quotes \
       -u sk_test_SECRET_KEY: \
       -d touchpoint_id=TOUCHPOINT_ID \
       -d item_id=ITEM_ID \
       -d language=de \
       -d data='{"duration":"P1Y","item_value":100000,"older_than_three_months":false,"theft":true,"damage":false,"loss":false}'

.. _QuoteResponse:

Sample quote response:

.. code:: javascript

    {
        "currency": "chf",
        "extra_data": {
            "deductible": 5000,
            "duration_prices": {
                "P1Y": 73200,
                "P2Y": 131400,
                "P3Y": 186200
            },
            "modules_prices": {
                "damage": 31000,
                "loss": 16800,
                "theft": 33500
            }
        },
        "gross_premium": 73200,
        "net_premium": 69714,
        "net_service_fee_total": 0,
        "premium_tax": 3486,
        "service_charge_vat": 0,
        "signature": "QUOTE_TOKEN",
        "tax_rate": 0.05
    }

.. _Offer:

Offer Data
-----------
JSON data posted to create an offer.

+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| Name                   | Description                  | Rules                                                                                                      | Example                              |
+========================+==============================+============================================================================================================+======================================+
| first_name             | Name of policy holder.       |  - ``required``, string any letters from ``EBCDIC 500``, special chars: ``space, dash, apostrophe``        | ``John``, ``Li-thylwar``,            |
|                        |                              |  - length ``min: 2``, ``max: 35`` chars                                                                    | ``Sit'redwarg``                      |
|                        |                              |  - space or dash or apostrophe are allowed but not as leading or trailing character                        |                                      |
|                        |                              |  - no subsequent space, dash, apostrophe                                                                   |                                      |
|                        |                              |  - no more than ``3 sequential chars``                                                                     |                                      |
|                        |                              |  - first character of each word should be in capitals                                                      |                                      |
|                        |                              |  - only upper case or lower case characters are not allowed                                                |                                      |
|                        |                              |  - keyword black list  Ref1_  - cannot contain specified keywords                                          |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| last_name              | Surname of policy holder.    |  - ``required``, string any letters from ``EBCDIC 500``, special chars: ``space, dash, apostrophe``        | ``Doe``, ``Armin van Buuren``        |
|                        |                              |  - length ``min: 2``, ``max: 35`` chars                                                                    |                                      |
|                        |                              |  - ``space, dash, apostrophe`` are allowed but not as leading or trailing character                        |                                      |
|                        |                              |  - no subsequent ``space, dash, apostrophe``                                                               |                                      |
|                        |                              |  - no more than ``3 sequential chars``                                                                     |                                      |
|                        |                              |  - first character of each word should be in capitals, except Ref2_ - specified names                      |                                      |
|                        |                              |  - only upper case or lower case characters are not allowed                                                |                                      |
|                        |                              |  - keyword black list Ref3_ -  - cannot contain specified keywords                                         |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| email                  | Email of policy holder.      |  - ``required``, valid email address, MX records are being checked                                         | ``john@example.com``                 |
|                        |                              |  - length ``min: 2``, ``max: 70`` chars                                                                    |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| language               | Policy holder language.      |   - ``required``, ``ISO 639-1``, supported languages: ``de, fr, ir``                                       | ``de``                               |
|                        |                              |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| phone                  | Policy holder phone number.  |  - ``required``, valid phone number, mobile or land line                                                   | ``+33677777777``                     |
|                        |                              |  - validation base on google's  https://github.com/google/libphonenumber                                   |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| salutation             | Customer title.              |  - ``required``, string                                                                                    | ``mr``, ``ms``                       |
|                        |                              |  - allowed values: ``mr``, ``ms``                                                                          |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| dob                    | Date of birth of the policy  |  - ``required``, ``ISO 8601`` date in format ``Y-m-d``                                                     | ``1989-02-04``                       |
|                        | holder.                      |  - at least 16 years old, not older than ``1900-01-01``                                                    |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| house_number           | House number of the          |  - ``required``, string, length ``max: 10``, alpha numeric chars and dash allowed                          | ``12``, ``45b``, ``k-2``             |
|                        | policyholder's address.      |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| street                 | Street name of the           |  - ``required``, string, length ``min 2``, ``max 30``, ``EBCDIC 500`` charset                              | ``Main street``                      |
|                        | policyholder's address.      |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| city                   | City of policyholder's       |  - ``required``, string, ``EBCDIC 500`` charset, no digits, each word capitalized                          | ``New York``, ``Berlin``             |
|                        | address.                     |  - length ``min 2``, ``max 30``,                                                                           |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| postcode               | Postcode of the              |  - ``required``, string, 4 symbols long, contains only digits                                              | ``1020``, ``9658``                   |
|                        | policyholder's address.      |  - should be one of allowed  Ref4_                                                                         |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| receipt_token          | Unique uploaded file         |  - ``optional`` string, valid receipt token, can be retrieved from request Receipt_                        | ``eyJpZCI6I.....``                   |
|                        | identifier.                  |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| item_identifier        | Unique item identifier       |  - ``required_without:receipt_token``, string                                                              | ``WT3211``                           |
|                        | (receipt number, IMEI etc.)  |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| item_description       | "Description of the item     |  - ``required``, string                                                                                    | ``Nikon D3000``                      |
|                        | (i.e. make + model).         |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| payee_private          | Flag determining flow and    |  - ``required``, string, Available values: ``private``, ``company``.                                       | ``company``                          |
|                        | clarifies who is paying.     |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| company_industry       | Company industry name.       |  - ``required_if,payee_private:company``, string                                                           | ``Architekturbüros``                 |
|                        |                              |                                                                                                            |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| company_legal_form     | Company legal form number.   |  - ``required_if,payee_private:company``, string                                                           | ``38``                               |
|                        |                              |  - one of ``10,15,20,21,22,23,29,30,31,32,33,34,36,38,40,41,50,55,60``                                     |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| company_name           | Company legal form number.   |  - ``required_if,payee_private:company``, string, length ``min: 3``, ``max: 70``                           | ``Kasko``                            |
|                        |                              |  - ``EBCDIC 500`` charset                                                                                  |                                      |
|                        |                              |  - cannot contain one of Ref5_ keywords                                                                    |                                      |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+
| company_uid            | Company unique identifier.   |  - ``optional``, string, should match regex  ``/^CHE-\\d{3}\\.\\d{3}\\.\\d{3}$/``                          | ``CHE-000.000.000``                  |
+------------------------+------------------------------+------------------------------------------------------------------------------------------------------------+--------------------------------------+


.. _Ref1:

*first_name* black list keyword reference ( comma delimited ):

.. literalinclude:: first_name_blacklist_keywords.py
    :language: python
    :linenos:
    :lines: 1-1

.. _Ref2:

*last_name*  each word capitals exceptions:

.. literalinclude:: last_name_each_word_capitalized.py
    :language: python
    :linenos:
    :lines: 1-1

.. _Ref3:

*last_name* black list keywords:

.. literalinclude:: last_name_blacklist_keywords.py
    :language: python
    :linenos:
    :lines: 1-1

.. _Ref4:

*postcode* list defined:

.. literalinclude:: allowed_postcodes.py
    :language: python
    :linenos:
    :lines: 1-1

.. _Ref5:

*company_name* keyword blacklist:

.. literalinclude:: company_negative_keywords.py
    :language: python
    :linenos:
    :lines: 1-1


Offer Request
~~~~~~~~~~~~~~~

.. code:: bash

   curl https://api.kasko.io/policies \
       -u sk_test_SECRET_KEY: \
       -d quote_token=QUOTE_TOKEN \
       -d first_name=John \
       -d last_name=Doe \
       -d email=john@example.com \
       -d language=de \
       -d data='{"phone":"+417304200","salutation":"mr","dob":"1989-02-04","house_number":"12","street":"Main street","city":"Lubeck","postcode":"1234","item_identifier":"WT3211","item_description":"Nikon D3000", "payee_private":"private"}'

.. _OfferResponse:

Sample offer response:

.. code:: javascript

    {
        "_links": {
            "_self": {
                "href": "https://api.kasko.io/policies/POLICY_ID"
            }
        },
        "id": "POLICY_ID",
        "insurer_external_policy_id": null,
        "insurer_policy_id": "TEST-BTTRY-XXXXX",
        "payment_token": "PAYMENT_TOKEN"
    }

.. _Receipt:

Receipt upload
--------------
Receipt upload allows to upload receipt file, that confirms purchase of specific product. This request is optional.
File should be less than ``5MB``. It can be one of: ``pdf``, ``gif``, ``jpeg``, ``gif`` formats.

.. code:: bash

    curl -X POST \
      https://api.kasko.io/services/baloise_receipt_upload \
          -u sk_test_SECRET_KEY: \
          -F "receipt=@/path/to/file/file.pdf"

Sample response

.. code:: json

    {
        "receipt_token": "<RECEIPT_TOKEN>"
    }

.. _Payment:

Convert offer to policy (payment)
---------------------------------

To create a policy you should convert offer to policy. In other words - make payment for the offer.
This can be done by making following request:

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``PAYMENT_TOKEN>`` returned by OfferResponse_."
   "policy_id", "yes", "``string``", "The 33 character long ``POLICY_ID`` returned by OfferResponse_."
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
            "method": "distributor",
            "provider": "distributor",
            "token": "PAYMENT_TOKEN",
            "policy_id": "POLICY_ID",
        }'

NOTE. You should use ``<POLICY ID>`` and ``<PAYMENT TOKEN>`` from OfferResponse_. After payment is made, policy creation is asynchronous.
