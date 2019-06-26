.. _kasko_js:

KASKO JS
========

KASKO JS is used to load and configure the KASKO widget onto your site.
KASKO JS automatically handles sizing of the widget so that it works
responsively in your page.

Before you start this guide you will need :ref:`keys`


Integration KASKO JS
-----------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>

Include the following JS snippet in the HEAD of the page or at above
where you want the widget to be created.

Widget Containter
--------------------

.. code:: html

    <div id="kasko-widget-container"></div>

Add a container div where you want the widget to be created on the page.

This can be given any ID but we have used kasko-widget-container in our
example below. This container can be set with any width, but should be
left “height:auto” so that KASKO JS can handle the height responsively.

Popup Mode
----------

If you wish to have the widget popup in a new window instead of loading inline on the page please add the foolowing to the Kasko.Setup method detailed below.

.. code:: html

    mode: "popup"

When using popup mode the containter attribute is the target which KASKO JS will attach the click event handler too.

Full Screen Mode
----------------

If you wish to have the widget to consume the whole page instead of loading inline on the page please add the follwoing to the Kasko.Setup method detailed below.

.. code:: html

    mode: "fullscreen"

Setup KASKO JS
--------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "TEST OR LIVE CLIENT KEY",
        product: "PRODUCT KEY",
        data: {
            optional data: "see optional params"
        }
      });
    </script>

Below the widget container div include the widget setup javascript

Parameters
~~~~~~~~~~

Widget setup parameters

+------------------+------------+---------------+---------------------------------------------------------+
| Parameter        | required   | Type          | Description                                             |
+==================+============+===============+=========================================================+
| container        | yes        | string        | ID of the div you want the KASKO Widget to appear in.   |
+------------------+------------+---------------+---------------------------------------------------------+
| key              | yes        | string        | TEST or LIVE client key provided by KASKO.              |
+------------------+------------+---------------+---------------------------------------------------------+
| product          | yes        | string        | Product key provided by KASKO.                          |
+------------------+------------+---------------+---------------------------------------------------------+
| data             | no         | data object   | Data                                                    |
+------------------+------------+---------------+---------------------------------------------------------+
| config           | no         | config object | Specific product configuration.                         |
+------------------+------------+---------------+---------------------------------------------------------+

Generic Optional data parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These fields can be provided to the data object.

These fields can prepopulate widget data or be used to pass extra information

+------------------+---------------+-----------------------------------------------------------------------------------------+
| Parameter        | Type          | Description                                                                             |
+==================+===============+=========================================================================================+
| first_name       | string        | Firstname of the customer - This will prepopulate in the widget                         |
+------------------+---------------+-----------------------------------------------------------------------------------------+
| last_name        | string        | Lastname of the customer - This will prepopulate in the widget                          |
+------------------+---------------+-----------------------------------------------------------------------------------------+
| email            | string        | Email Address of the customer - This will prepopulate in the widget                     |
+------------------+---------------+-----------------------------------------------------------------------------------------+

.. note::   Please see product specific page for product specific optional data params.

Generic Optional config parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These fields configure the application depending on the needs of the page.

+-----------------------+----------+--------------------------------------------------------------------------------+
| Parameter             | Type     | Description                                                                    |
+=======================+==========+================================================================================+
| header\_visibility    | string[] | On what devices should the header be visible? Defaults to ALL devices.         |
+-----------------------+----------+--------------------------------------------------------------------------------+
| footer\_visibility    | string[] | On what devices should the footer be visible? Defaults to desktop and tablet.  |
+-----------------------+----------+--------------------------------------------------------------------------------+
| hamburger\_visibility | string[] | On what devices should the hamburger side menu be visible? Defaults to mobile. |
+-----------------------+----------+--------------------------------------------------------------------------------+

Available device types: `desktop`, `tablet`, `mobile`. If no device type is defined (`[]` - empty array), then this section will not be visible on any device.

.. note::   Please see product specific page for product specific optional config params.

Testing
-------

Once the Widget is working in TEST mode, you can buy a policy with the
following CC details

+----------------------+--------------------------+
| Field                | Detail                   |
+======================+==========================+
| Credit Card Number   | 4111 1111 1111 1111      |
+----------------------+--------------------------+
| CVC                  | 123                      |
+----------------------+--------------------------+
| Exp                  | 12/19                    |
+----------------------+--------------------------+
| Name                 | Any name above 4 chars   |
+----------------------+--------------------------+

Please contact techsupport@kasko.io with the URL of your page for us to
check the integration

Go Live
-------

When testing is complete and you're ready to Go Live, please swap the
Client TEST key for the Client LIVE key in your production site.

.. note:: You must swap you client key with the LIVE client key before going live.

Querystring Prefill
-------------------

Sometimes it's useful to prefill a webapp with predefined data. For example, an email campaign may have a link to the webapp integration. In order to store the email campaign tracker ID on the customer's policy, query string prefill can be used.

.. note::   ?kdata=eyJmaXJzdF9uYW1lIjoiSm9obiJ9

`kdata` is short for `KASKO data`. This querystring parameter is used to prefill an application with given `data` (name, address, email, etc) and `metadata` (could be anything, but most commonly used for analytics tracking data or agent information).

.. warning::   `kdata` can only be used on the integration level. It will not work if set on webapp level (`webapp.kasko.io` domain). This is because KASKO JS is responsible for decoding `kdata` and passing it on to the webapp in a different format.

`kdata` value can be a url-safe-base64-encoded string or a JSON string. **It is preferred to use url-safe-base64-encoded string as it is supported by all browsers.**


Examples
~~~~~~~~

url-safe-base64-encoded string (only data):

.. code:: html

    ?kdata=eyJmaXJzdF9uYW1lIjoiSm9obiJ9


url-safe-base64-encoded string (data + metadata):

.. code:: html

    ?kdata=eyJkYXRhIjp7ImZpcnN0X25hbWUiOiJKb2huIn0sIm1ldGFkYXRhIjp7ImFnZW50X2lkIjoxMjN9fQ


JSON string (only data):

.. code:: html

    ?kdata={"first_name":"John"}


JSON string (data + metadata):

.. code:: html

    ?kdata={"data":{"first_name":"John"},"metadata":{"agent_id":123}}


.. note::   What's *url-safe-base-encoded string*? This is a base64 encoded string that has all the trailing equals signs removed from it.


Limitations
~~~~~~~~~~~

Some older browsers have strict max URL length limits after which the URL gets truncated. If this limit is breached, the base64 or JSON value gets broken. In general it is recommended to have the URL length below 2000 characters long. Read `this StackOverflow explanation for more information <https://stackoverflow.com/a/417184>`_.
