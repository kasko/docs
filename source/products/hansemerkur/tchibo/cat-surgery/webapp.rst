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
        touchpoint: "tp_d0f608514295b1a6f2ae07e769b34",
        mode: "fullscreen",
        language: "de",
        config: {
          adnr_number: "{adnr_number}"
        }
      });
    </script>
