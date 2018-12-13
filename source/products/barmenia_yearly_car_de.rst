Kfz-Versicherung: Premium-Schutz
================================

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "Kfz-Versicherung: Premium-Schutz"
   "Description", "German yearly car cover"
   "ID", "KJAR5gL1GY0j6QDGbrM72z3v9yoO8VkB"
   "Language", "DE"
   "Region", "DE"
   "Live", "True"


Preload Data
------------
This data should be prefilled in the KASKO JS to preload the insurance widget with data.

Example data object
~~~~~~~~~~~~~~~~~~~

.. code:: html

          data: {
              customer: {
                  first_name: 'Matthew',
                  last_name: 'Wardle',
                  email: 'test@kasko.io',
                  phone: '3124891841',
                  city: 'München',
                  street: 'Flößergasse ',
                  home_number: '200',
                  postcode: '81373'
              },
              car: {
                  name: 'BMW Z4',
                  number: 'WBALL31050J246912',
                  registration_date: '2012-05-01',
                  mileage: '63500',
                  hsn: '0005',
                  tsn: 'BGX'
              }
          }

Parameters
~~~~~~~~~~

.. csv-table::
   :header: "Name", "Type", "Description", "Example Value"
   :widths: 20, 20, 80, 20

   "first_name", "string", "customer first name", "Matthew"
   "last_name", "string", "customer last name", "Wardle"
   "email", "string", "customer email", "test@kasko.io"
   "phone", "string", "customer phone", "3124891841"
   "city", "string", "customer city", "München"
   "street", "string", "customer street", "Flößergasse"
   "home_number", "string", "customer street number", "200"
   "postcode", "string", "customer zip code", "81373"
   "name", "string", "car name", "BMW Z4"
   "number", "string", "car FIN", "WBALL31050J246912"
   "registration_date", "string", "car registration date", "2012-05-01"
   "mileage", "string", "car mileage", "63500"
   "hsn", "string", "car HSN", "0005"
   "tsn", "string", "car TSN", "BGX"


KASKO JS Setup object example
-----------------------------

.. code:: html

      Kasko.Setup({
        container: "target-button",
        mode: "popup",
        key: "TEST OR LIVE CLIENT KEY of distirbutor",
        product : 'KJAR5gL1GY0j6QDGbrM72z3v9yoO8VkB',
        overlay_color: "#FFFFFF",
          theme: "barmenia-carzada",
          data: {
              customer: {
                  first_name: 'Matthew',
                  last_name: 'Wardle',
                  email: 'test@kasko.io',
                  phone: '3124891841',
                  city: 'München',
                  street: 'Flößergasse ',
                  home_number: '200',
                  postcode: '81373'
              },
              car: {
                  name: 'BMW Z4',
                  number: 'WBALL31050J246912',
                  registration_date: '2012-05-01',
                  mileage: '63500',
                  hsn: '0005',
                  tsn: 'BGX'
              }
          }
      });

