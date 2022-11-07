========
SFTP API
========

The KASKO SFTP API is used for purchasing policies by providing a CSV file that contains policy data.

.. note::  To access the KASKO SFTP server, a username and SSH key are required. Please contact techsupport@kasko.io.

Getting started
---------------

.. note:: You must have FTP client (like FileZilla) installed on you environment.
`FileZilla <http://www.filezilla-project.org/>`_


There are only two steps involved to create policies.

1) Create a CSV file, which contains all necessary data, that are needed to create policies.

2) Place this file in the SFTP test or live folders (corresponding mode policies will be created)

Once the first file is processed, you will find the ``imports_processed`` folder on the same level as the test and live folders.
It will contain the same files, you placed in previously (their are moved here after processing is done. It may take a few minutes).

Policies, that are not transmutable, will be reported through a webhook call with the error message.

CSV file
--------------

There are some rules, that must be followed when creating the file.
The first row in the CSV file is the header. It maps each value to the corresponding field.

To create a policy, you must include Quote and Policy fields, which are described by the product itself.

For example, a person's name belongs to the policy data, so it would look like this ``policy.first_name``.
Any data, that are related to the quote fields are defined the same way ``quote.data.pem_exclusion``.

You must include an idempotency key.
The value of this column must be unique and it is used to distinguish duplicates. We recommend using ``UUID4 (universally unique identifier)``.
You can generate UUID4 here https://www.uuidgenerator.net/version4 . But it can be anything else, as long as it is unique.

For more details about data fields visit Ticketplan API documentation https://developers.kasko.io/en/latest/products/ticketplan/ticket-refund/rest-api.html.

Example CSV content
--------------------
.. csv-table::
   :header: "idempotency_key", "integration_id", "quote.data.policy_start_date", "quote.data.pem_exclusion", "quote.data.sports_inclusion", "quote.data.protected_element.1.price", "policy.first_name", "policy.last_name", "policy.email", "policy.language", "policy.data.booking_date", "policy.data.payment_date", "policy.data.ticket_quantity", "policy.data.order_number", "policy.data.order_value", "policy.data.order_currency", "policy.data.event_name", "policy.data.event_start_date", "policy.data.event_end_date", "policy.data.venue_name", "policy.data.venue_location", "policy.data.venue_country", "policy.data.ticket_distributor", "policy.data.customer_email", "policy.data.customer_first_name", "policy.data.customer_last_name", "policy.data.customer_house_number", "policy.data.customer_street", "policy.data.customer_city", "policy.data.customer_postcode", "policy.data.protected_elements_value", "policy.data.unprotected_elements_value", "policy.data.insurance_quantity"

   "c93dc6de-08f7-4551-ab0e-ea19e5fccb2c", "in_41c2f22d51612f04ee2c48eca3dc0", "2022-11-11", "1",  "0", "3000", "Nick", "Malone", "nick.malone@com", "en", "2022-10-02", "2022-10-02", "1", "5597", "100000", "euro", "test event", "2022-12-02", "2022-12-02", "venue name", "Test location", "Test country", "Tickeplan distributor", "customer.email@test.com", "Nick", "Malone", "123", "test customer street", "test customer city", "44444", "55555", "55555", "1"
   "48467ef6-06f0-40b6-afe2-b387e13cd680", "in_41c2f22d51612f04ee2c48eca3dc0", "2022-12-03", "1",  "0", "3000", "John", "Smith", "john.smith@com", "en", "2022-10-02", "2022-10-02", "1", "5597", "100000", "euro", "test event", "2022-12-02", "2022-12-02", "venue name", "Test location", "Test country", "Tickeplan distributor", "customer.email@test.com", "John", "Smith", "123", "test customer street", "test customer city", "44444", "55555", "55555", "1"

* example excel: :download:`file <../files/ticketplan_import.xlsx>`

When the import file is ready, you must convert it to the CSV file and then place in the SFTP.





