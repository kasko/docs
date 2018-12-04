Webapp
======

KASKO JS Data Parameters
------------------------

Optional data that can be put in the ``data`` tag of the KASKO embed object.

.. csv-table::
   :header: "Name", "Required", "Type", "Description", "Example Value"

   "product",  "``no``", "`string`", "Safe Pay, Safe Surf, Surf & Pay", "``1`` - Safe pay, ``2`` - Safe surf, ``3`` - Safe pay & surf."
   "family",   "``no``", "`string`", "Single, Family",                  "``1`` - single, ``2`` - family."
   "duration", "``no``", "`string`", "Duration in years",               "``PY1`` - single year, ``PY2`` - two years, ``PY3`` - three years."

Example KASKO JS embed code
---------------------------

.. code:: html

    <script>
      Kasko.Setup({
        container: "kasko-widget-container",
        key: "KEY",
        touchpoint: "tp_38efa11a75798629ec4bd4084a5c9",
        mode: "fullscreen",
        language: "de",
      });
    </script>



