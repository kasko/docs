OCC AutoScout24 Quote Lookup
============================

This API is used for getting an indicitive quote for a AutoScout24 Make and Model

Getting started
---------------

The basic steps to creating a policy are as follows.

1) Make GET request to URL

2) If quote is successful show quote to customer


Get a Quote
-----------
Get a indicitive quote from the KASKO platform

Definition
~~~~~~~~~~
.. code:: bash

	GET https://api.kasko.io/indexes/PGqXYJndRax9zoNzAEjMLwA01y76B5gp

Parameters
~~~~~~~~~~

+------------------+------------+---------------+----------------------------------------------------------------+
| Parameter        | required   | Type          | Description                                                    |
+==================+============+===============+================================================================+
| year             | yes        | integer       |  year of car production                                        |
+------------------+------------+---------------+----------------------------------------------------------------+
| make             | yes        | string        |  AS24 car make                                                 |
+------------------+------------+---------------+----------------------------------------------------------------+
| model            | yes        | string        |  AS24 car model                                                |
+------------------+------------+---------------+----------------------------------------------------------------+
| market value     | yes        | integer       |  AS24 car market value                                         |
+------------------+------------+---------------+----------------------------------------------------------------+

.. csv-table::
   :header: "Parameter", "required", "Type", "Description"
   :widths: 20, 20, 20, 80

   "year", "yes", "integer", "Year of car production"
   "make", "yes", "string", "AS24 car make"
   "model", "yes", "string", "AS24 car model"
   "market value ", "yes", "integer", "AS24 car market value"



Example Request
~~~~~~~~~~~~~~~

.. code:: bash

	curl 'https://api.kasko.io/indexes/PGqXYJndRax9zoNzAEjMLwA01y76B5gp?year=1990&make=Peugeot&model=205&market_value=30000'

Example Response Success
~~~~~~~~~~~~~~~~~~~~~~~~

response code 200

.. code:: javascript

	{
	  "quote_price": "212.43",
	  "deductable": "150"
	}

Example Response Failure
~~~~~~~~~~~~~~~~~~~~~~~~

response code 404

.. code:: javascript

	{
	  "message": "Not Found",
	  "description": "Quote could not be found"
	}



If you have any questions please email us at techsupport@kasko.io



