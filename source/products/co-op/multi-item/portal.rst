Portal
======

KASKO JS is used to load and configure the KASKO widget onto your site.
It is required to include KASKO JS in your page for the snippet below to work. More about this can be found in :ref:`kasko_js`.

When implementing, you will need :ref:`keys`.

Example Kasko JS embed code
---------------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        app: "portal",
        key: "KEY",
        language: "en",
        config: {
          webapp_url: "https://your.webapp.com/link/to/page"
        }
      });
    </script>
