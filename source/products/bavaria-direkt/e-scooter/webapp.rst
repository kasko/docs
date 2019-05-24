Webapp
======

Example Kasko JS embed code
---------------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: 'kasko-widget-container',
        key: 'KEY',
        touchpoint : 'tp_dee807120dd72aa0628cd5eb0de89',
        mode: 'fullscreen',
        language: 'de',
        config: {
          hamburger_visibility: ['desktop', 'tablet', 'mobile'],
          header_visibility: ['desktop', 'tablet', 'mobile'],
          footer_visibility: [],
        },
      });
    </script>
