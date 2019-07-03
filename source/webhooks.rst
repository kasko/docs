.. _webhooks:

Webhooks
========

Webhooks allow you to build and set up custom scripts which subscribe to certain events. When one of those events is
triggered, we'll send a HTTP POST payload to the webhook's configured URL. Webhooks can be used to be notified about
events that happen in your KASKO account. You're only limited by your imagination.

Each webhook can be installed on an account level, and each account can have multiple webhooks, with different sets of
events configured. You can also choose between ``live`` and ``test`` events.

Configuring webhooks
--------------------

In order to create a webhook please contact ``techsupport@kasko.io``.

Receiving a webhook notification
--------------------------------

Creating a webhook endpoint on your server is no different from creating any page on your website. With PHP, you might
create a new .php file on your server; with a framework like Sinatra, you would add a new route with the desired URL.

Webhook data is sent as JSON in the ``POST`` request body. The full event details are included and can be used directly,
after parsing the JSON into an Event object.

Each webhook request made by KASKO will additionally include request headers outlined below.

.. csv-table::
   :header: "Name", "Example Value", "Description"
   :widths: 20, 20, 80

   "``X-KASKO-Event``", "``policy.created``", "The event type that was triggered."
   "``X-KASKO-Delivery``", "``baf59150f9a60c1c0e6a700e6f531676``", "A `guid <https://en.wikipedia.org/wiki/Universally_unique_identifier>`_ to identify the payload and event being sent."
   "``X-KASKO-Signature``", "``sha1=88dd4948bf8fc1e991a574179fc668c76d329db2``", "The value of this header is computed as the HMAC hex digest of the payload, using the webhook secret as the key."

.. code:: php

    // Retrieve the request body and parse it as JSON
    $input = @file_get_contents("php://input");
    $event_json = json_decode($input);

    // Do something with $event_json

    http_response_code(200); // PHP 5.4 or greater

Responding to a webhook
-----------------------

To acknowledge receipt of a webhook, your endpoint should return a ``2xx`` HTTP status code. Any other information
returned in the request headers or request body is ignored. All response codes outside this range, including ``3xx``
codes, will indicate to KASKO that you did not receive the webhook. This does mean that a URL redirection or a
"Not Modified" response will be treated as a failure.

Checking signatures
-------------------

KASKO will sign the webhook events it sends to your endpoint, allowing you to validate that they were not sent
by a third-party. You can verify signatures using simple hashing algorithm. To make use of signatures, you first
need to retrieve your endpoint’s secret (email us for more details). This signature is included in the
``X-KASKO-Signature`` header, and sent along with the event. The value of this header is computed as the HMAC hex
digest of the body, using the webhook secret as the key.

.. code:: php

    $endpoint_secret = "whsec_...";

    $payload = @file_get_contents("php://input");
    $sig_header = $_SERVER["HTTP_X_KASKO_SIGNATURE"];

    $hash = 'sha1=' . hash_hmac('sha1', $payload, $endpoint_secret);

    if ($hash !== $sig_header) {
        // Then the request did not came from KASKO and should be dropped.
    }

    // Do something with $event

    http_response_code(200); // PHP 5.4 or greater


Events
------

Policy created
~~~~~~~~~~~~~~

This event is triggered when policy has been successfully created.

**Payload**

.. code:: javascript

    {
      "id": "tJGmCVljXbLr1kn1BzpAIgOxegK3AMBoy",
      "livemode": false,
      "policy_id": "TEST-POLICY-12345",
      "product_id": "oH68at2ddL7WnDJg23jhMPGRJy83QNpa",
      "first_name": "John",
      "last_name": "Doe",
      "email": "john@example.com",
      "policy_created_date": "2017-07-18T12:37:56+00:00",
      "start_date": "2017-07-18T12:37:56+00:00",
      "end_date": "2017-08-12T12:37:56+00:00",
      "gross_premium": 1999,
      "premium_tax": 400,
      "metadata": {
        "some": "value"
      },
      "assets": [
        {
          "designation": "policy_document",
          "name": "Policy Certificate",
          "extension": "pdf",
          "href": "https://assets.kasko.io/test/policies/private/tJGmCVljXbLr1kn1BzpAIgOxegK3AMBoy/TEST-POLICY-12345/Document.pdf"
        }
      ],
      "_links": {
        "self": {
          "href": "https://api.kasko.io/policies/tJGmCVljXbLr1kn1BzpAIgOxegK3AMBoy"
        }
      }
    }

Policy updated
~~~~~~~~~~~~~~

This event is triggered when policy has been successfully updated.

**Payload**

.. code:: javascript

    {
      "id": "tJGmCVljXbLr1kn1BzpAIgOxegK3AMBoy",
      "livemode": false,
      "policy_id": "TEST-POLICY-12345",
      "product_id": "oH68at2ddL7WnDJg23jhMPGRJy83QNpa",
      "first_name": "John",
      "last_name": "Doe",
      "email": "john@example.com",
      "policy_created_date": "2017-07-18T12:37:56+00:00",
      "start_date": "2017-07-18T12:37:56+00:00",
      "end_date": "2017-08-12T12:37:56+00:00",
      "gross_premium": 1999,
      "premium_tax": 400,
      "metadata": {
        "some": "value"
      },
      "assets": [
        {
          "designation": "policy_document",
          "name": "Policy Certificate",
          "extension": "pdf",
          "href": "https://assets.kasko.io/test/policies/private/tJGmCVljXbLr1kn1BzpAIgOxegK3AMBoy/TEST-POLICY-12345/Document.pdf"
        }
      ],
      "_links": {
        "self": {
          "href": "https://api.kasko.io/policies/tJGmCVljXbLr1kn1BzpAIgOxegK3AMBoy"
        }
      }
    }

