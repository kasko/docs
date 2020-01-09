.. _domain_data:

========
Data API
========

When to use it
==============

You would use the Data API when you have an array of customer domain data and want to do any of the following things:

- allow the webapp user to choose a value for some field from a predefined set, not adding another value to the set;

- validate the user input against a predefined set of values both on frontend and backend;

- populate some more fields based on the user’s choice in another field;

- allow to choose values for different fields, thus filtering and narrowing down the choices for the following fields belonging to the same set of data;

- limit the availability of the whole set to a certain account;

- limit the availability of certain fields to frontend.


How to use it
=============

A typical usage flow:

- The frontend app requests a list of items for a dropdown to display on the field with the dataset rule from the Data API, specifying the dataset fields;

- The Data API responds with a list of item IDs and values for the chosen dataset fields;

- User selects a value;

- The frontend app requests a single dataset item by its ID;

- The API responds with the full set of public fields for the item;

- The frontend app submits a dataset item ID in the payload for the field instead of a set of fields (e.g. company data) to the quote or policy API, e.g. ``"some_field”: ”dai_some_dataset_item_id________”``.


How to retrieve the data
========================

.. note:: All of the retrieval samples below show unauthenticated requests for publicly available datasets.

When you need to access a dataset locked to an account, add an ``Authentication: Bearer YOUR_TOKEN`` header to the request.


Get a dataset information record
--------------------------------

Definition
~~~~~~~~~~

.. code:: rest

    GET https://api.kasko.io/datasets/DATASET_ID

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl 'https://api.kasko.io/datasets/DATASET_ID'

Example output
~~~~~~~~~~~~~~

.. code:: json

    {
        "id": "dat_abcxyz______________________",
        "name": "Some dataset name",
        "languages": ["en", "de"]
    }


List items from a set
---------------------

Definition
~~~~~~~~~~

.. code:: rest

    GET https://api.kasko.io/datasets/DATASET_ID/dataset_items

Parameters
~~~~~~~~~~

.. csv-table::
    :header: "Parameter", "Required", "Type", "Description"

    "``language``", "yes", "string", "Language"
    "``fields``", "yes", "JSON array of strings", "Fields to output in the ``value`` property"
    "``limit``", "no", "integer", "Number of items per page, 1 to 100, default = 20"
    "``starting_after``", "no", "string", "Item ID to start the page after"
    "``ending_before``", "no", "string", "Item ID to end the page before"
    "``query``", "no", "JSON array of objects", "Search query"
    "``sort``", "no", "JSON array of objects", "Sorting order"

The ``query`` parameter format is ``[{"field": "FIELD", "search": "VALUE", "operator": "OPTIONAL_OPERATOR"}]``.

The ``search`` field of the ``query`` parameter supports ``%`` wildcards; the accepted operators are ``>``, ``>=``, ``<``, ``<=``, and ``IN``. The type of the value is array for the ``IN`` operator and string for everything else.

Sample ``search`` value:

.. code:: json

    [
      {"field": "name", "search": "Kas%"},
      {"field": "price", "operator": ">", "search": "10"},
      {"field": "category", "operator": "IN", "search": ["a", "b"]}
    ]

The ``sort`` parameter format is ``[{"field": "FIELD", "dir": "asc|desc"}]``.

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl 'https://api.kasko.io/datasets/DATASET_ID/dataset_items' \
        -d language=de \
        -d fields=["name","code"]

Example output
~~~~~~~~~~~~~~

.. code:: json

    [
        {
            "id": "dai_abcxyz",
            "value": {
                "name": "Kasko",
                "code": "Z1",
            }
        },
        ...
    ]

The ``Link`` header holds the links for the next and previous page queries.


List distinct values of a field in a dataset
--------------------------------------------

Definition
~~~~~~~~~~

.. code:: rest

    GET /dataset/DATASET_ID/items_distinct_values


Parameters
~~~~~~~~~~

.. csv-table::
    :header: "Parameter", "Required", "Type", "Description"

    "``language``", "yes", "string", "Language"
    "``fields``", "yes", "JSON array of strings", "Fields to output; when multiple fields are requested, the response is distinct sets of fields"
    "``limit``", "no", "integer", "Number of items per page, 1 to 100, default = 20"
    "``starting_after``", "no", "integer", "Distinct value number to start the page after"
    "``ending_before``", "no", "integer", "Distinct value number to end the page before"
    "``query``", "no", "JSON array of objects", "Search query, same as in the ``/dataset_items`` request"
    "``sort``", "no", "JSON array of objects", "Sorting order, same as in the ``/dataset_items`` request"


Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl 'https://api.kasko.io/datasets/DATASET_ID/items_distinct_values' \
        -d language=de \
        -d fields=["name","code"]

Example output
~~~~~~~~~~~~~~

.. code:: json

    [
        {
            "name": "Kasko",
            "code": "Z1",
        },
        ...
    ]

The ``Link`` header holds the links for the next and previous page queries.


Get one item
------------

Definition
~~~~~~~~~~

.. code:: rest

    GET https://api.kasko.io/dataset_items/ITEM_ID


Parameters
~~~~~~~~~~

.. csv-table::
    :header: "Parameter", "Required", "Type", "Description"

    "``language``", "yes", "string", "Language"

Example Request
~~~~~~~~~~~~~~~

.. code:: bash

    curl 'https://api.kasko.io/dataset_items/ITEM_ID' \
        -d language=de

Example output
~~~~~~~~~~~~~~

.. code:: json

    {
        "id": "dai_abcxyz",
        "data": {
            "field_name": "field_value",
            ...
        }
    }


Sending data to backend
=======================

Instead of the literal values in the dataset item you send the item ID.

Example Request For a Quote
---------------------------

.. code:: rest

    GET https://api.kasko.io/quote?data=
    {
      ...
      "company": "dai_123_some_item_id_____________",
      ...
    }

The backend service transparently converts this input to the full item available to use in pricing, policy, etc.
