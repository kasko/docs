.. _rest_api:

REST API
===================

The KASKO REST API is primarily used for getting quotes and purchasing policies.

.. note::  Your account has to be API enabled before it can access the API.   Please contact techsupport@kasko.io if you would like access.


Getting started
---------------

The basic steps to creating a policy are as follows.

1) Set Authorization headers for all requests

2) Get a Quote

3) Create a Policy

4) Convert a policy status to paid

5) Testing and Go-Live

.. note:: While developing/testing please ensure that you use the TEST key.

	Once the implementation has been completed and tested, and a go-live date has been approved by the business, you can change you key to the LIVE key.


Authentication
--------------

Authenticate your account when using the API by including your secret API :ref:`key <keys>` in the request. Your API keys carry many privileges, so be sure to keep them secret! Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.

Requests that require authentication will return ``404 Not Found``, instead of ``403 Forbidden``, in some places. This is to prevent the accidental leakage of sensitive data to unauthorized users.

Authentication to the API is performed via `HTTP Basic Auth <https://en.wikipedia.org/wiki/Basic_access_authentication>`_. Provide your API key as the basic auth username value. You do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use ``-H "Authorization: Bearer sk_test_SECRET_KEY"`` instead of ``-u sk_test_SECRET_KEY:``.

All API requests must be made over `HTTPS <https://en.wikipedia.org/wiki/HTTPS>`_. Calls made over plain HTTP will fail.

**Basic Authentication**

.. code:: rest

	curl -u sk_test_SECRET_KEY: https://api.kasko.io

**Bearer Authentication**

.. code:: rest

	curl -H "Authorization: Bearer sk_test_SECRET_KEY" https://api.kasko.io


Errors
------

KASKO uses conventional `HTTP response <https://en.wikipedia.org/wiki/List_of_HTTP_status_codes>`_ codes to indicate the success or failure of an API request.
In general, codes in the ``2xx`` range indicate success,
codes in the ``4xx`` range indicate an error that failed given the information provided (e.g., a required parameter was omitted, etc.),
and codes in the ``5xx`` range indicate an error with KASKO's servers (these are rare).

.. csv-table::
   :header: "Code", "Description"

   "200 OK", "Everything worked as expected."
   "400 Bad Request", "The request was unacceptable, often due to missing a required parameter."
   "401 Unauthorized", "No valid API key provided."
   "403 Forbidden", "The API key is valid, but does not have enough permissions."
   "404 Not Found", "The requested resource doesn't exist."
   "500 Internal Server Error", "Something went wrong on KASKO's end. (These are rare.)"
   "503 Service Unavailable", "Something went wrong on KASKO's end. (These are rare.)"

Request IDs
-----------

Each API request has an associated request identifier. You can find this value in the response headers, under ``X-Request-Id``.

.. hint::
   If you need to contact us about a specific request, providing the request identifier will ensure the fastest possible resolution.

Get a Quote
--------------------
Get a quote from the KASKO platform.

Depending if you are using an old product (pre 2018) or a new product (post 2018), there will be a different approach to this step.

- :ref:`Old Products - Get Quote <old_rest_products>`
- :ref:`New Products - Get Quote <new_rest_products>`

Create unpaid policy
--------------------
Create policy on KASKO platform

Depending if you are using v1 policy or v2 policy, there will be a different approach to this step

- :ref:`Create unpaid v1 policy <create_unpaid_v1_policy>`
- :ref:`Create unapid v2 policy <create_unpaid_v2_policy>`

Get specific policy
--------------------

Depending wether you are using v1 policy or v2 policy, there will be a different approach to this step.

- :ref:`Get v1 policy <get_v1_policy>`
- :ref:`Get v2 policy <get_v2_policy>`


Convert unpaid policy to paid policy
---------------------------------------------

Convert an already created policy to a paid policy.

This API request should be made after payment has been taken for the policy by the distributor.   This will trigger the KASKO platform to email the customer the Policy Documents.

Definition
~~~~~~~~~~
.. code:: bash

	POST https://api.kasko.io/payments


Parameters
~~~~~~~~~~

+------------------+------------+---------------+---------------------------------------------------------+
| Parameter        | required   | Type          | Description                                             |
+==================+============+===============+=========================================================+
| payment_token    | yes        | string        |  Payment token from /policy endpoint                    |
+------------------+------------+---------------+---------------------------------------------------------+
| policy_id        | yes        | string        |  Policy ID from /policy endpoint                        |
+------------------+------------+---------------+---------------------------------------------------------+

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

	curl 'https://api.kasko.io/payments' \
	    -u sk_test_SECRET_KEY: \
	    -d token=PAYMENT_TOKEN \
	    -d policy_id=POLICY_ID


Fetch additional data
---------------------

When a product's web application, quote, or policy requires some additional domain-related data, use the :ref:`domain_data`.

Testing
-------

Please contact techsupport@kasko.io with the URL of your page for us to check the integration

Go Live
----------

When testing is complete and you're ready to Go Live, please swap the
Client TEST key for the Client LIVE key in your production site.

.. note:: You must swap you client key with the live client key before going live.

