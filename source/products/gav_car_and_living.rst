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
                  tsn: 'BGX'
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


KASKO JS Setup object example
------------------------------------

.. code:: html

      Kasko.Setup({
        container: "iframe-container",
        key: "TEST OR LIVE CLIENT KEY of distributor",
        product : 'LJROpwloaQ8ZBmMAewM7W5PyE4qvAb31',
        overlay_color: "#FFFFFF",
          data: {
              car: {
                  make: 'BMW',
                  model: 'x1',
                  first_reg_year: '2010',
                  first_reg_month: '7',
                  mileage: '7500',
                  hsn: '5',
                  tsn: 'BGX'
              }
          }
      });

