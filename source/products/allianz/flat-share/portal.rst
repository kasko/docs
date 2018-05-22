Portal
======

KASKO JS is used to load and configure the KASKO widget onto your site.
It is required to include KASKO JS in your page for the snippet below to work. More about this can be found in :ref:`kasko_js`.

When implementing, you will need :ref:`keys`.

Example Kasko JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        app: "portal",
        key: "KEY",
        language: "de"
      });
    </script>
