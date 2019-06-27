Webapp
======

Example Kasko JS embed code
---------------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint: "tp_e2775e2996362bdaab169b8a74d12",
        mode: "fullscreen",
        language: "de",
        config: {
          adnr_number: {adnr_number},
        }
      });
    </script>
