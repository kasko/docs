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
        touchpoint: "tp_14a00f394fe37d85348bce9908907",
        mode: "fullscreen",
        language: 'de',
        metadata: {
          tan: response.tan,
        }
      })
    </script>
