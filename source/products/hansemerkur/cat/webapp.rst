Webapp
======

Example Kasko JS embed code
---------------------------

.. code:: html

    <script type="text/javascript" src="https://eu1.kaskojs.com/v2"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint: "tp_0d65cd1ebdecb251627b360770269",
        mode: "fullscreen",
        language: "de",
        config: {
          adnr_number: "{adnr_number}"
        }
      });
    </script>
