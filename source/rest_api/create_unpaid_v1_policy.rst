.. _create_unpaid_v1_policy:

Create an unpaid v1 policy
---------------------------------
Create an unpaid policy on the Kasko platform.

Why do we do this 2 step mechanism?   We want to ensure that there is no problem with the quote or details before you charge your customers for the policy.  After this point we can ensure that the policy can be purchased.

Definition
~~~~~~~~~~
.. code:: bash

	POST https://api.kasko.io/policies


Parameters
~~~~~~~~~~
+----------------------------+------------+---------------+---------------------------------------------------------+
| Parameter                  | required   | Type          | Description                                             |
+============================+============+===============+=========================================================+
| quote_token                | yes        | string        |  Quote token provided in quote object                   |
+----------------------------+------------+---------------+---------------------------------------------------------+
| first_name                 | yes        | string        |  First Name of the customer                             |
+----------------------------+------------+---------------+---------------------------------------------------------+
| last_name                  | yes        | string        |  Last Name of the customer                              |
+----------------------------+------------+---------------+---------------------------------------------------------+
| email                      | yes        | string        |  Email address of the customer                          |
+----------------------------+------------+---------------+---------------------------------------------------------+
| language                   | yes        | string        |  Two letter ISO 639‑1 language code                     |
+----------------------------+------------+---------------+---------------------------------------------------------+
| distributor_traffic_source | no         | string        |  Distributor reference of traffic source.  e.g. Email   |
+----------------------------+------------+---------------+---------------------------------------------------------+
| data                       | maybe      | JSON          |  Refer to the product section here for required data    |
+----------------------------+------------+---------------+---------------------------------------------------------+

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

	curl https://api.kasko.io/policies \
	    -u sk_test_SECRET_KEY: \
	    -d quote_token=QUOTE_TOKEN \
	    -d first_name=FIRSTNAME \
	    -d last_name=SURNAME \
	    -d email=EMAIL_ADDRESS \
	    -d data=DATA



Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

	{
	  "id": "tmGgyzWx47B5qY6wXMLPNREA9dDnOQVZ3",
	  "payment_token": "2pwqBTy+79gK/dKuJmRjC1yTk7jx5zvuh5tn34139GiOd8irZuuTB6ViTKyRMNW8VcctGzDAn+QQf9fHOjdowpE67GHEFFuy4X+QFfx87qlg=",
	  "_links": {
	    "_self": {
	      "href": "https://api.kasko.io/policies/tmGgyzWx47B5qY6wXMLPNREA9dDnOQVZ3"
	    }
	  }
	}
