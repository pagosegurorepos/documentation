# Integración API Tokenización (Pagos Recurrentes)

## Introducción

Pago Seguro te ofrece integración API rest para conectar sus serviciós de pagos recurrentes de manera directa sin necesidad de acceder a nuestro portal web y tener control total de los cobros de manera interna.

## Configuración

Para realizar conexiones API mediante carros de compra hacia la plataforma de Pago Seguro, se debe tener el comercio creado y habilitado en la modalidad del servicio carros de compra, con el fin de poder obtener el `Account ID` y el `Api key`.
Dicha información se encuentra en el menú de herramientas, carros de compra tanto para el acceso de pruebas (sandbox) como para el acceso de producción.

![install](../img/api/tokenization/menu_access.png)

![install](../img/api/tokenization/menu_tokenization.png)

Una vez obtenga el `Account ID` y `Api Key` para poder autenticarse en nuestro servicio y obtener el TOKEN que le permitirar ejecutar transacciones con el carro de compras

## Respuesta API

[Respuestas API](/api/autentication.md?id=respuesta-api)

## Autenticación

[Autenticación](/api/autentication.md?id=autenticación)

## Datos a Enviar

### Diccionario de datos

| CAMPO                   | TIPO DE DATO       | TAMAÑO | DESCRIPCIÓN                                                                                        |                                  EJEMPLO                                  |
| ----------------------- | ------------------ | :----: | -------------------------------------------------------------------------------------------------- | :-----------------------------------------------------------------------: |
| key                     | int                |        | Account ID, se encuentra en la plataforma de Pagos                                                 |                                 123456789                                 |
| txnid                   | string             |   20   | Referencia de compra                                                                               |                                  SO0001                                   |
| amount                  | decimal            |   13   | Monto total de la compra, se admiten decimales con separador decimal " . "                         |                                 90000.99                                  |
| productinfo             | string             |   40   | Descripción general de la venta                                                                    |                               Producto Uno                                |
| firstname               | string             |   40   | Nombre completo del comprador incluido apellidos                                                   |                                 Jhon Doe                                  |
| email                   | string(email)      |        | Email del comprador                                                                                |                            jhondoe@compra.com                             |
| hash                    | string             |        | Cadena hash en sha512 el cual concatena los valores (txnid, amount, productinfo, firstname, email) |              [Ver Hash](/api/integrationCheckout.md?id=hash)              |
| quota                   | int - between:1,36 |        | Cantidad de coutas                                                                                 |                                    12                                     |
| brand                   | string             |        | Franquicia de la tarjeta de crédito                                                                |                                Mastercard                                 |
| expiryMonth             | int - between:1,12 |   2    | Mes expiración de la tarjeta de crédito                                                            |                                     6                                     |
| expiryYear              | int                |   2    | Año expiración de la tarjeta de crédito                                                            |                                    21                                     |
| card                    | string             |   2    | Número de tarjeta y código encriptados                                                             | [Ver Encriptar Tarjeta](/api/integrationCheckout.md?id=encriptar-tarjeta) |
| number                  | int                |        | Número de la tarjeta de crédito                                                                    |                             4000000000000000                              |
| code                    | int                |  3/4   | Código de seguridad de la tarjeta de crédito                                                       |                                    000                                    |
| typeIdentification      | int - between:1,3  |        | Tipo de documento del pagador:<br>1 = Cédula Ciudadanía<br>2 = Cédula Extranjería<br>3 = Pasaporte |                                     1                                     |
| billingNumberIdentifier | int                |   20   | Número de documento del pagador                                                                    |                                 100000000                                 |
| billingAddress          | string             |        | Dirección del pagador                                                                              |                            Calle luna # 10-99                             |
| billingNames            | string             |        | Nombres del pagador                                                                                |                                   Jhon                                    |
| billingLastNames        | string             |        | Apellidos del pagador                                                                              |                                    Doe                                    |
| billingEmail            | string(email)      |        | Email del pagador                                                                                  |                            jhondoe@pagador.com                            |
| callerPhoneNumber       | int                |  7/10  | Teléfono ó Celular del pagador                                                                     |                                3100000000                                 |
| city                    | string             |        | Ciudad de Residencia del pagador                                                                   |                                  Bogotá                                   |

### Hash

El hash es una cadena de texto la cual se debe enviar mediante método POST junto con la demás información, esta es una cadena de seguridad la cual se debe cifrar mediante sha512 dicha cadena de texto debe cumplir con el siguiente parámetro:

Ejemplo

```php
hash('sha512', $key . '|' . $txnid . '|' . $amount . '|' . $productinfo . '|' . $firstname . '|' . $email . '|' . '/payment/process||||||||||' . \$api_key)
```

En donde se concatenan las variables generarles de la venta junto con el Api key y el Account ID:

- \$key: Account id del comercio, obtenido desde la plataforma de Pago Seguro Colombia

- \$txnid: Referencia de la compra

- \$amount: Monto total de la compra

- \$productinfo: Descripción general de la venta

- \$firstname: Nombre y apellidos del comprador

- \$email: email del comprador

- \$api_key: Api key del comercio, obtenido desde la plataforma de Pago Seguro Colombia

### Encriptar Tarjeta

<table>
    <tbody>
        <tr>
            <th>METODO</th>
            <td colspan="2">POST</td>
        </tr>
        <tr>
            <th>URL PRODUCCIÓN</th>
            <td colspan="2">https://www.autorizaciones-pagoseguro.com/pagosegurocanal/public/api/v1/hashcard</td>
        </tr>
        <tr>
            <th>URL PRUEBAS</th>
            <td colspan="2"> https://www.sanbox-pagoseguro.com/pagosegurocanal/public/api/v1/hashcard</td>
        </tr>
        <tr>
            <th rowspan="2">HEADERS</th>
            <td colspan="2">
               Content-Type: application/json
            </td>
        </tr>
        <tr>
            <td colspan="2">
               Authorization: Bearer Token
            </td>
        </tr>
        <tr>
            <td colspan="3">
            </td>
        </tr>
        <tr>
            <th rowspan="3">BODY</th>
            <td class="text-right">
                <strong>number:</strong>
            </td>
            <td>
                Número de la tarjeta de crédito
            </td>
        </tr>
        <tr>
            <td class="text-right">
                <strong>code:</strong>
            </td>
            <td>
                Código de seguridad de la tarjeta de crédito
            </td>
        </tr>
        <tr>
            <td colspan="2">
                Ejemplo:
                <br>
                <code>
                {<br>
                    "number": 4000000000000000,<br>
                    "code": "000"<br>
                }
                </code>
            </td>
        </tr>
    </tbody>
</table>

?> Ejemplo CURL
```curl
curl --location --request POST \
'https://www.autorizaciones-pagoseguro.com/pagosegurocanal/public/api/v1/hashcard' \
--header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpc3MiOiJodHRwOlwvXC81Mi44Ni4zMy4yMTQ6MzAwMFwvcGFnb3NlZ3Vyb2NhbmFsXC9wdWJsaWNcL2FwaVwvYXV0aFwvYWNjb3VudGxvZ2luIiwiaWF0IjoxNTg4MDgzMTA5LCJleHAiOjE1ODgwODY3MDksIm5iZiI6MTU4ODA4MzEwOSwianRpIjoib1pRcWNmUThIR0ZwYWZNdSIsInN1YiI6NTIsInBydiI6IjIzYmQ1Yzg5NDlmNjAwYWRiMzllNzAxYzQwMDg3MmRiN2E1OTc2ZjcifQ.ShslkgdY85TJhd6KDVgbxWwLAtw3O9a1_LsPcqiQwZl-tfvaQTo7xoSwBx_EEyL1iQBbHc55LQKw-BC-yNHJeQ' \
--header 'Content-Type: application/json' \
--data-raw '{
    "number": 4000000000000000,
    "code": 000
}'
```

?> Respuesta

| PROPIEDAD | TIPO DE DATO | DESCRIPCIÓN                              |
| --------- | :----------- | ---------------------------------------- |
| **data**  | `string`     | El "data" valor de la tarjeta encriptado |

?> Ejemplo Encriptación Correcta
```javascript
{
    "success": true,
    "data": "eyJpdiI6Im5kbk5NQXI5Z3pzQlhqcGYzQmxReGc9PSIsInZhbHVlIjoiT1MxVjhPK1lYajJoV1cramdscFcxNTJHSGZlblZXajBcL2VmTUJSSFhmXC9zPSIsIm1hYyI6ImExY2IyYzM1MGI5Y2UwNDg4ZGE4OGMzMmQzZjNmN2Y4NTJmZTdkZTc0N2IxMDYyODBlOTk1ZmM3M2QzMmM0NjIifQ==",
    "status_code": 200
}
```

!> Ejemplo Encriptación Errónea
```javascript
{
    "success": false,
    "message": "Token Vencido, Genere uno Nuevo",
    "status_code": 401
}
```

## Ejecutar Pago

<table>
    <tbody>
        <tr>
            <th>METODO</th>
            <td colspan="2">POST</td>
        </tr>
        <tr>
            <th>URL PRODUCCIÓN</th>
            <td colspan="2">https://www.autorizaciones-pagoseguro.com/pagosegurocanal/public/api/v1/tokenization</td>
        </tr>
        <tr>
            <th>URL PRUEBAS</th>
            <td colspan="2"> https://www.sanbox-pagoseguro.com/pagosegurocanal/public/api/v1/tokenization</td>
        </tr>
        <tr>
            <th rowspan="2">HEADERS</th>
            <td colspan="2">
               Content-Type: application/json
            </td>
        </tr>
        <tr>
            <td colspan="2">
               Authorization: Bearer Token
            </td>
        </tr>
        <tr>
            <td colspan="3">
            </td>
        </tr>
        <tr>
            <th>BODY</th>
            <td colspan="2">
                Ejemplo:
                <br>
                <code>
                {<br>
                "key": 1000000000,<br>
                "txnid": "QOITHSVLB",<br>
                "amount": 32900,<br>
                "productinfo": "Suscripción Canal",<br>
                "firstname": "Jhon Doe",<br>
                "email": "jhondoe@compra.com",<br>
                "hash": "72f4e83ae9a7e5556d6d785f2ca7fc01c5ff87a8b2b5e4f0acccb0af29f044d6fcca670c2101cb7ea64a24e6ed08e3bbfc7067e3182d0aedc96c7c699763be01",<br>
                "quota": 1,<br>
                "brand": "MasterCard",<br>
                "expiryMonth": 6,<br>
                "expiryYear": 21,<br>
                "card": "eyJpdiI6IlppRGxWVjRzNHk4elRjMTJiQTZwMXc9PSIsInZhbHVlIjoiSmR4OUdXYXJ0cWIyVFoxSXJkR2xlcmJnU2dPN05IZTBRR2JWbytlOGtLaz0iLCJtYWMiOiI1ZmJiOGE4MTc5ZjE1ZDU2ZDY0MTNiMTVkOGYzMzI2NTNkNmM5NjkxNDE1MjRhMjU0YWMwZWMwZDkzM2FlMzJlIn0=",<br>
                "typeIdentification": 1,<br>
                "billingNumberIdentifier": "100000000",<br>
                "billingAddress": "Calle luna # 10-99",<br>
                "billingNames": "Jhon",<br>
                "billingLastNames": "Doe",<br>
                "billingEmail": "jhondoe@pagador.com",<br>
                "callerPhoneNumber": 5550000,<br>
                "city": "Bogotá"<br>
            }
                </code>
            </td>
        </tr>
    </tbody>
</table>

?> Ejemplo CURL
```curl
curl --location --request POST \ 
'https://www.autorizaciones-pagoseguro.com/pagosegurocanal/public/api/v1/tokenization' \
--header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpc3MiOiJodHRwOlwvXC81Mi44Ni4zMy4yMTQ6MzAwMFwvcGFnb3NlZ3Vyb2NhbmFsXC9wdWJsaWNcL2FwaVwvYXV0aFwvYWNjb3VudGxvZ2luIiwiaWF0IjoxNTkwNzYzMDc3LCJleHAiOjE1OTA3NjY2NzcsIm5iZiI6MTU5MDc2MzA3NywianRpIjoiOFFVN0ZHaHRlYm02SkxvZiIsInN1YiI6NTIsInBydiI6IjIzYmQ1Yzg5NDlmNjAwYWRiMzllNzAxYzQwMDg3MmRiN2E1OTc2ZjcifQ.Bsy5ivJcIUyuwbdiyh6UYrVGYEoger5s7rf_HDnu_9Wr_HtDyfWH8AzzBejiQrfD0HkLtMC-Ar3Z78vaq9BHqg' \
--header 'Content-Type: application/json' \
--data-raw '{
    "key": 1000000000,
    "txnid": "QOITHSVLB",
    "amount": 32900,
    "productinfo": "Suscripción Canal",
    "firstname": "Jhon Doe",
    "email": "jhondoe@compra.com",
    "hash": "72f4e83ae9a7e5556d6d785f2ca7fc01c5ff87a8b2b5e4f0acccb0af29f044d6fcca670c2101cb7ea64a24e6ed08e3bbfc7067e3182d0aedc96c7c699763be01",
    "quota": 1,
    "brand": "MasterCard",
    "expiryMonth": 6,
    "expiryYear": 21,
    "card": "eyJpdiI6IlppRGxWVjRzNHk4elRjMTJiQTZwMXc9PSIsInZhbHVlIjoiSmR4OUdXYXJ0cWIyVFoxSXJkR2xlcmJnU2dPN05IZTBRR2JWbytlOGtLaz0iLCJtYWMiOiI1ZmJiOGE4MTc5ZjE1ZDU2ZDY0MTNiMTVkOGYzMzI2NTNkNmM5NjkxNDE1MjRhMjU0YWMwZWMwZDkzM2FlMzJlIn0=",
    "typeIdentification": 1,
    "billingNumberIdentifier": "100000000",
    "billingAddress": "Calle luna # 10-99",
    "billingNames": "Jhon",
    "billingLastNames": "Doe",
    "billingEmail": "jhondoe@pagador.com",
    "callerPhoneNumber": 5550000,
    "city": "Bogotá"
}'
```

?> Respuesta

| PROPIEDAD                 | TIPO DE DATO | DESCRIPCIÓN                                                        |
| ------------------------- | :----------: | ------------------------------------------------------------------ |
| **data**                  | `object {}`  | El "data" representa si el resultado del llamado api fue correcto. |
| data.**status**           |   `string`   | Resultado de la transacción: Aprobado o Rechazado                  |
| data.**code**             |   `string`   | Código de ejecución                                                |
| data.**amount**           |   `doble`    | Valor de la transacción                                            |
| data.**transaction_code** |    `int`     | Código de la transacción                                           |
| data.**date**             |   `string`   | Fecha y hora de la transacción                                     |

?> Ejemplo Encriptación Correcta
```javascript
{
    "success": true,
    "data": {
        "status": "Aprobado",
        "code": 836158,
        "amount": 36000,
        "transaction_code": 16526683,
        "date": "2020-05-29T10:28:13"
    },
    "status_code": 200
}
```

!> Ejemplo Pago Erróneo
```javascript
{
    "success": false,
    "message": "Rechazo General",
    "status_code": 500
}
```
