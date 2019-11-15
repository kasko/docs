Makler Webapp
=============

Example Kasko JS embed code
---------------------------

.. code:: html

   <script type="text/javascript" src="https://eu1.kaskojs.com/v2"></script>
    <div id="kasko-widget-container"></div>
   <script>
      Kasko.Setup({
         container: "kasko-widget-container",
         key: "<KEY>>",
         touchpoint: "tp_48a637ddc8b1e03df97bf41144323",
         mode: "fullscreen",
         language: 'de'
      });
   </script>
