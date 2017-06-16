GAV Car & Living
===================

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "Car & Living"
   "Description", " "
   "ID", "LJROpwloaQ8ZBmMAewM7W5PyE4qvAb31"
   "Language", "DE"
   "Region", "DE"


Preload Data
------------
This data should be prefilled in the KASKO JS to preload the insurance widget with data.

Example data object
~~~~~~~~~~~~~~~~~~~~~~

.. code:: html

          data: {
              car: {
                  make: 'BMW',
                  model: 'x1',
                  first_reg_year: '2010',
                  first_reg_month: '7',
                  mileage: '7500',
                  hsn: '5',
                  tsn: 'BGX',
                  article_id: '1ab123'
              }
          }

Parameters
~~~~~~~~~~

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"

   "make", "string", "car make", "BMW"
   "model", "string", "car model", "x1"
   "first_reg_year", "string", "car first registration year", "2010"
   "first_reg_month", "string", "car first registration month", "7"
   "mileage", "string", "car mileage", "7500"
   "hsn", "string", "car HSN", "5"
   "tsn", "string", "car TSN", "BGX"
   "article_id", "string", "article identifier", "1ab123"

OnSuccess callback
------------------
Upon successful customer payment the `onSuccess` callback function is called with the policy details. This is an optional paramater.

Callback arguments
~~~~~~~~~~~~~~~~~~
The `onSuccess` parameter function has only one argument - the policy details object. Please note that `price` is the price in cents (i.e. 9900 is 99.00 EUR).

.. code:: json
   
   {
      "id": "unique_policy_id",
      "currency": "eur",
      "price": 9900,
      "policy_name": "Car & Living",
      "policy_details": "Die Reparaturkostenversicherung der GAV"
   }


KASKO JS Setup object example
------------------------------------

.. code:: html

      Kasko.Setup({
        container: "iframe-container",
        key: "TEST OR LIVE CLIENT KEY of distributor",
        product : 'LJROpwloaQ8ZBmMAewM7W5PyE4qvAb31',
        overlay_color: "#FFFFFF",
        onSuccess: function (data) {
            console.log(data);
        },
        data: {
            car: {
                make: 'BMW',
                model: 'x1',
                first_reg_year: '2010',
                first_reg_month: '7',
                mileage: '7500',
                hsn: '5',
                tsn: 'BGX',
                article_id: '1ab123'
            }
        }
      });

