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
        touchpoint : "tp_9a54f296194f5a1bafe971322570a",
        mode: "fullscreen",
        language: "de",
        config: {
            portal_button_url: "https://www.kasko.io/"
        }
      });
    </script>
