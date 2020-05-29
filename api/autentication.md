# Integración API Pago Seguro

## Introducción

Pago Seguro te ofrece integración API rest para conectar sus serviciós de manera directa sin necesidad de acceder a nuestro portal web y tener control total de los cobros de manera interna.

## Respuesta API

| PROPIEDAD     |   TIPO DE DATO         |DESCRIPCIÓN                                                                                         |
|---------------|:----------------------:|----------------------------------------------------------------------------------------------------|
|**success**     |`bool (true or false)` |El "success" representa si el resultado del llamado api fue correcto o hubó un error al ejecutarlo  |
|**data**<br>ó<br>**message** |`data: object {}`<br><br>`message: string\|object {}`| El "data" representa si el resultado del llamado api fue correcto.<br><br>El "message" representa si el resultado del llamado api fue incorrecto.|
|**status_code** |`int (200, 400, 404, 500, ...)`| El "status_code" representa si el resultado del llamado api fue correcto o hubó un error al ejecutarlo. Para ejecuciones correctas siempre devolverá 200|

?> Ejemplo Respuesta Correcta
```javascript
{
    "success": true,
    "data": {},
    "status_code": 200
}

```
!> Ejemplo Error
```javascript
{
    "success": false,
    "message": "",
    "status_code": 400
}
```

## Autenticación

<table>
    <tbody>
        <tr>
            <th>METODO</th>
            <td colspan="2">POST</td>
        </tr>
        <tr>
            <th>URL PRODUCCIÓN</th>
            <td colspan="2">https://www.autorizaciones-pagoseguro.com/pagosegurocanal/public/api/auth/accountlogin</td>
        </tr>
        <tr>
            <th>URL PRUEBAS</th>
            <td colspan="2"> https://www.sanbox-pagoseguro.com/pagosegurocanal/public/api/auth/accountlogin</td>
        </tr>
        <tr>
            <th>HEADERS</th>
            <td colspan="2">
               Content-Type: application/json
            </td>
        </tr>
        <tr>
            <th rowspan="3">BODY</th>
            <td class="text-right">
                <strong>account_id:</strong>
            </td>
            <td>
                Account ID del comercio
            </td>
        </tr>
        <tr>
            <td class="text-right">
                <strong>api_key:</strong>
            </td>
            <td>
                Api Key del comercio
            </td>
        </tr>
        <tr>
            <td colspan="2">
                Ejemplo:
                <br>
                <code>
                {<br>
                    "account_id": 123456789,<br>
                    "api_key": "xxxxxxxxxxxxxxxxxx"<br>
                }
                </code>
            </td>
        </tr>
    </tbody>
</table>

?> Ejemplo CURL
```curl
curl --location --request POST \
'https://www.autorizaciones-pagoseguro.com/pagosegurocanal/public/api/auth/accountlogin' \
--header 'Content-Type: application/json' \
--data-raw '{
	"account_id": 123456789,
	"api_key": "xxxxxxxxxxxxxxxxxx"
}'
```

?> Respuesta

| PROPIEDAD           | TIPO DE DATO | DESCRIPCIÓN                                                                             |
|---------------------|:------------:|-----------------------------------------------------------------------------------------|
|**data**             |`object {}`   |El "data" representa si el resultado del llamado api fue correcto.                       |
|data.**access_token**|`string`      |Token de autenticación                                                                   |
|data.**token_type**  |`string`      |Tipo de autenticación (bearer)                                                           |
|data.**expires_in**  |`int`         |Tiempo de expiración del Token en segundos (3600)                                        |

?> Ejemplo Autenticación Correcta
```javascript
{
    "success": true,
    "data": {
        "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpc3MiOiJodHRwOlwvXC81Mi44Ni4zMy4yMTQ6MzAwMFwvcGFnb3NlZ3Vyb2NhbmFsXC9wdWJsaWNcL2FwaVwvYXV0aFwvYWNjb3VudGxvZ2luIiwiaWF0IjoxNTkwNjEyNTQxLCJleHAiOjE1OTA2MTYxNDEsIm5iZiI6MTU5MDYxMjU0MSwianRpIjoiWXdWc0thdHNNZFBQYnFmSSIsInN1YiI6NTIsInBydiI6IjIzYmQ1Yzg5NDlmNjAwYWRiMzllNzAxYzQwMDg3MmRiN2E1OTc2ZjcifQ.Goa4IKvCmNomlruwZTzmydjc5tkhsC9_ujwFjAm2KoRGd22Zv9MoNs4ZsPqSYMHKLfoa6ZyiUBbbpZEVwOwRzA",
        "token_type": "bearer",
        "expires_in": 3600
    },
    "status_code": 200
}
```

!> Ejemplo Autenticación Errónea
```javascript
{
    "success": false,
    "message": "Account Id y/o API Key Invalidos",
    "status_code": 400
}
```