Webapp
======

Example Kasko JS embed code
---------------------------

.. code:: html

    <script type="text/javascript" src="https://eu1.kaskojs.com/v2"></script>
    <div id="kasko-widget-container"></div>
    <script>
      window.onmessage = function(event) {
        if (event.data && (event.data.kaskoQuery || event.data.kaskoKdata)) {
          const agentNumber = event.data.kaskoQuery;
          const kaskoKdata = event.data.kaskoKdata;
          const kaskoSetup = {
            container: "kasko-widget-container",
            key: "KEY",
            touchpoint: "tp_5485e420133b5dd5af6acb60261bc",
            mode: "fullscreen",
            language: "fr",
            callback: {
              pagechange: function() {
                window.scrollTo(0,0);
              }
            }
          };
          if (kaskoKdata) {
            const kdata = atob(kaskoKdata);
            const config = JSON.parse(kdata).config;
            kaskoSetup.config = config;
          }
          if (agentNumber && agentNumber !== "undefined") {
            kaskoSetup.data = {
              agent_number: agentNumber,
            };
          }
          return Kasko.Setup(kaskoSetup);
        }
      };
    </script>