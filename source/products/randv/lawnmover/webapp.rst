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
        touchpoint : "tp_67c15784e8affc5ef2f4203d2be60",
        mode: "fullscreen",
        language: "de"
      });
    </script>
======

Example Kasko JS embed code with preselected item
-------------------------------------------------

.. code:: html

    <script type="text/javascript" src="https://js.kasko.io"></script>
    <div id="kasko-widget-container"></div>
    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint : "tp_67c15784e8affc5ef2f4203d2be60",
        mode: "fullscreen",
        language: "de",
        config: {
            item_id: "item_e386830222162143735cee21f29"
        }
      });
    </script>
