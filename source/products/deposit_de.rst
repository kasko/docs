Mietkautions­Bürgschaft
=======================

Product Information
-------------------

.. csv-table::
   :widths: 50, 50

   "Name", "Mietkautions­Bürgschaft"
   "Description", "German deposit cover"
   "Insurer", "R+V"
   "ID", "d2leL7QmGaOyz4M3RBxPgrkVBEqK6RJZ"
   "Language", "DE"
   "Region", "DE"

Kasko JS Optional Data Parameters
---------------------------------
Optional data that can be put in the "data" tag of the kasko embed object

.. csv-table::
   :header: "Name", "Description", "Example Value"

   "first_name",  "Customer first name", "John"
   "last_name",  "Customer last name", "Smith"
   "salutation",  "Customer title", "mr || ms"
   "registration",  "Customer title", "x"
   "email",  "Customer email address", "name@example.com"
   "dob",  "Customer date of birth", "1976-05-19 // yyyy-mm-dd"
   "street",  "Customer Street", "CustStreet"
   "house_number",  "Customer house number", "99"
   "postal_code",  "Customer postal code", "01067"
   "deposit_amount",  "Deposit ammount in cents", "150000 (euro cents)"
   "monthly_rent",  "Monthly rent in cents", "60000 (euro cents)"
   "landlord_salutation",  "Landlord title", "mr || ms || firm"
   "landlord_firstname",  "Landlord first name", "LandFirst"
   "landlord_lastname",  "Landlord last name", "LandLast"
   "landlord_firm",  "Landlord firm name", "LandFirm"
   "landlord_firm_more",  "Landlord firm additional", "LandFirm+"
   "landlord_street",  "Landlord street", "LandStreet"
   "landlord_house_number",  "Landlord house number", "1"
   "landlord_postal_code",  "Landlord post code", "01069"
   "post_documents_to",  "Send documents to tennant or landlord", "landlord // landlord || tenant"
   "property_street",  "Property street name", "PropStreet"
   "property_house_number",  "Property house number", "1"
   "property_postal_code",  "Property post code", "99998"


Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "iframe-container",
        key: 'KEY',
        product : 'd2leL7QmGaOyz4M3RBxPgrkVBEqK6RJZ',
        overlay_color: "#FFFFFF",
        data: {
             'first_name'        : 'Passy',
             'last_name'         : 'Passer',
             'salutation'        : 'mr',
             'email'             : 'name@example.com',
             'dob'               : '1976-05-19',
             'street'            : 'CustStreet',
             'house_number'      : '99',
             'postal_code'       : '01067',
             'deposit_amount'    : '150000',
             'monthly_rent'      : '60000',
             'landlord_salutation'   : 'mr',
             'landlord_firstname'    : 'LandFirst',
             'landlord_lastname'     : 'LandLast',
             'landlord_firm'         : 'LandFirm',
             'landlord_firm_more'    : 'LandFirm+',
             'landlord_street'       : 'LandStreet',
             'landlord_house_number' : '1',
             'landlord_postal_code'  : '01069',
             'post_documents_to'     : 'landlord',
             'property_street'       : 'PropStreet',
             'property_house_number' : '2',
             'property_postal_code'  : '99998',
        }
      });
    </script>



