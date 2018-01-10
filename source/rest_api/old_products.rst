.. _old_rest_products:

Get a Quote
--------------------
Get a quote from the KASKO platform

Definition
~~~~~~~~~~
.. code:: bash

	GET https://api.kasko.io/quotes

Parameters
~~~~~~~~~~

+------------------+------------+---------------+----------------------------------------------------------------+
| Parameter        | required   | Type          | Description                                                    |
+==================+============+===============+================================================================+
| variant_id       | yes        | string        |  Variant key provided by KASKO.                                |
+------------------+------------+---------------+----------------------------------------------------------------+
| data             | yes        | JSON          |  Refer to the :ref:`products` section for required data        |
+------------------+------------+---------------+----------------------------------------------------------------+

.. note:: Please URL Encode data

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

	curl --get https://api.kasko.io/quotes \
	    -u sk_test_SECRET_KEY: \
	    --data variant_id=VARIANT_ID \
	    --data-urlencode data=DATA

Example Response
~~~~~~~~~~~~~~~~

.. code:: javascript

	{
	  "gross_premium": 699,
	  "currency": "eur",
	  "net_premium": 587,
	  "net_service_fee_total": 0,
	  "premium_tax": 112,
	  "service_charge_vat": 0,
	  "signature": "2zDDsM+hPro2cDOhEqj7RuwJzcxcwC/YgG2Wim13AFLaQXqhAL7hPDFTm5qhGV9wWm9dwinvcd44DnB22v6D1oYQmvM18MrKZtQZzoGb1Qtn8cH90ZIaKeywrxyNopZFOgw61PBbF74qo4Z1E4LKrbjEVl8fD9OJXcukDnC2/r7Yi7KkEIGhKkBUyjn4LMlupi6rfpMUjRtx73f5WWin8lGJTGRIdcJGZKArE53wVZZKIRt230ee6ZXUOkGlPkKD7iJ15qOTCmKeoeaYY8+h59WT2Vmm6HSlljTuu11/a1nwLz9rjmYIN9GOewQKuWXW0gL1xUuJh0cmGd8rMBjZ74FlhS59YxkSUzJJ4bsfE6cmcRXylBdb6iMG5WDryN4hpaTs8gqx9O8iphCTfpRox0l1LNYjJWdX7gaFHYkW7ZeI8HsFQs/Dc4QYTfOTud6Xzu5k25Ae51z/AOyNZBk0T3RSByYnKFzv/czm19UzbdPU="
	}
