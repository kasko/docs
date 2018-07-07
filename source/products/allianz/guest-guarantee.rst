Allianz Guest Guarantee  API
======================================

This API is used for creation of Allianz Guest Guarantee policies.

Getting started
---------------

This uses the KASKO push API and is a single request.

Authentication
--------------

Authenticate your account when using the API by including your secret API :ref:`key <keys>` in the request. Your API keys carry many privileges, so be sure to keep them secret! Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.

Requests that require authentication will return ``404 Not Found``, instead of ``403 Forbidden``, in some places. This is to prevent the accidental leakage of sensitive data to unauthorized users.

Authentication to the API is performed via `HTTP Basic Auth <https://en.wikipedia.org/wiki/Basic_access_authentication>`_. Provide your API key as the basic auth username value. You do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use ``-H "Authorization: Bearer sk_test_SECRET_KEY"`` instead of ``-u sk_test_SECRET_KEY:``.

All API requests must be made over `HTTPS <https://en.wikipedia.org/wiki/HTTPS>`_. Calls made over plain HTTP will fail.


Header
------

All requests must be in JSON and include ``Content-Type`` header.

.. code:: rest

	-H "Content-Type: application/json"

REST API
========

Get Quote Request
-----------------

Data fields
^^^^^^^^^^^

Query string data appended to the quote request

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "checkin_date", "yes", "``string``", "Check-in date in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD). Must be greater than or equal to todays date."
   "checkout_date", "yes", "``string``", "Check-out date in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format (YYYY-MM-DD). Must be greater than or equal to check-in date."
   "sum_insured", "yes", "``string``", "The sum insured in cents. Allowed values are ``100000``, ``200000`` or ``300000``"

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/quotes \
      -u <YOUR SECRET API KEY>: \
      -d variant_id=d7zoBRlEp9yar6XyrjxPWm05VqwkQKA8 \
      -d data='{"checkin_date":"2018-10-05","checkout_date":"2018-10-08","sum_insured":"200000"}'

Create Unpaid Policy Request
----------------------------

Data fields
^^^^^^^^^^^

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "street", "yes", "``string``", "Customers street."
   "house_number", "yes", "``string``", "Customers house number."
   "postcode", "yes", "``string``", "Customers postal code."
   "city", "yes", "``string``", "Customers city."
   "country", "yes", "``string``", "Customers country."

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

  curl https://api.kasko.io/policies \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "quote_token": "<TOKEN OBTAINED FORM QUOTE REQUEST>",
            "first_name": "Matthew",
            "last_name": "Wardle",
            "email": "mwardle@kasko.io",
            "data": {
                "street":"2nd Avenue",
                "house_number":"123",
                "postcode":"UX XXX",
                "city":"Gotham",
                "country":"UK"
            }
        }'

Convert Policy To Paid Request
------------------------------

After creating unpaid policy it is required to convert it to paid. This can be done by making another request.

Data fields
^^^^^^^^^^^

.. csv-table::
   :header: "Parameter", "Required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "token",     "yes", "``string``", "The ``payment_token`` returned by the create policy request."
   "policy_id", "yes", "``string``", "The 33 character long policy ID returned by the create policy request."

Example Request
^^^^^^^^^^^^^^^

.. code:: bash

    curl https://api.kasko.io/payments \
        -X POST \
        -u <YOUR SECRET API KEY>: \
        -H 'Content-Type: application/json' \
        -d '{
            "token": "<PAYMENT TOKEN>",
            "policy_id": "<ID OF THE POLICY>"
        }'

