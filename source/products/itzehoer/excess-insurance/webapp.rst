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
        touchpoint : "TOUCHPOINT",
        mode: "fullscreen",
        language: "de"
      });
    </script>
