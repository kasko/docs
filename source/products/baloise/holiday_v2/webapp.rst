Webapp
======

Kasko JS Optional Parameters
----------------------------

Optional data that can be put in the ``data`` tag of the KASKO embed object.

.. csv-table::
   :header: "Name", "Required", "Type", "Description", "Example Value"

   "luggage_value",  "``no``", "`int`",  "The value of luggage. Increments of ``1000`` starting from ``0``, up to ``20000``", "``0``, ``1000``, ``2000``, .., ``20000``"
   "reduced_excess", "``no``", "`bool`", "Indicates whether reduced excess is included.", "``true`` or ``fase``"

Kasko JS ``metadata`` Parameters 
--------------------------------

Optional data that can be put in the ``metadata`` tag of the kasko embed object.

.. csv-table::
   :header: "Name", "Required", "Type", "Description", "Example Value"

   "salesagent_id", "``no``", "`string`", "The ID of the sales agent.", "``123456789``"

Example Kasko JS embed code
---------------------------

.. code:: html

    <script type="text/javascript" src="https://eu1.kaskojs.com/v2"></script>
    <div id="kasko-widget-container"></div>
    <script>
        Kasko.Setup({
            container: "kasko-widget-container",
            key: "KEY",
            touchpoint: "tp_a7461d7cf4b247fc6b472e2abc41d",
            mode: "fullscreen",
            language: 'de',
            data: {
              luggage_value: 3000,
              reduced_excess: true
            },
            metadata: {
              salesagent_id: "123456789"
            }
        });
    </script>
