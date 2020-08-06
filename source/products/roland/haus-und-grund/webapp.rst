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
        touchpoint: "tp_14a00f394fe37d85348bce9908907",
        mode: "fullscreen",
        language: 'de',
        metadata: {
          tan: response.tan,
        },
        data: {
          "rental_module":true,
          "rental_number":3,
          "duplex_houses":[
            {
              "duplex_city":"Ort eins",
              "duplex_house_number":"1",
              "duplex_postcode":"12121",
              "duplex_selected":true,
              "duplex_street":"Erster Weg"
            },
            {
              "duplex_city":"Ort zwei",
              "duplex_house_number":"2",
              "duplex_postcode":"13131",
              "duplex_selected":true,
              "duplex_street":"Zweiter Weg"
            },
            {
              "duplex_city":"Ort drei",
              "duplex_house_number":"3",
              "duplex_postcode":"14141",
              "duplex_selected":true,
              "duplex_street":"Dritter Weg"
            }
          ],
          "street":"Entengasse",
          "house_number":"13",
          "postcode":"13131",
          "city":"Entenhausen",
          "living_selected":false,
          "gender":"ms",
          "dob":"1992-02-02",
          "phone":"234234",
          "first_name":"Daisy",
          "last_name":"Duck",
          "email":"test@kasko.io"
        }
      })
    </script>
