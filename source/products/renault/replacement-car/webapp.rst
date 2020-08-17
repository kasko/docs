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
            touchpoint: "tp_d85f7dc5906cd4eb410ff8578349a",
            mode: "fullscreen",
            language: 'de'
        });
    </script>
