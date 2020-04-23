# Descripcion del servicio

Las integraciones mediante webcheckout permiten que un cliente seleccione cualquier cantidad de ítems tanto de productos como servicios desde su tienda ecommerce, al momento de realizar la compra es redireccionado a la pasarela de pagos de Pago Seguro en donde se deben diligenciar los datos de la información correspondiente a la transacción.

La información enviada hacia el canal de pagos de nuestra pasarela es realizada mediante el método POST HTTP por el cual se reciben los datos mediante el request que se envié desde su tienda ecommerce.

# Componentes del servicio

- Formulario de pago comercio:

  Este formulario se encuentra en la pagina web del comercio, el cual contiene datos generales de la venta como por ejemplo datos del cliente, precio total de la venta incluido el impuesto, descripción corta de la venta, referencia, etc.

- Web Checkout Formulario de pago Pago Seguro

  Luego de diligenciar el formulario anterior el cliente deberá ser redireccionado a el formulario de pagos establecido por Pago Seguro, en este formulario se registran algunos datos necesarios para la transacción y la información de pago.

- Página de respuesta

  Es una pagina donde se identifica la respuesta de la transacción, si fue aprobada o rechazada por parte del canal transaccional.

- Página de confirmación del cliente

  Pagina de respuesta del comercio en la cual se debe identificar cualquier tipo de mensaje dependiendo si la transacción fue aprobada o rechazada.

# Proceso del servicio

1. El primer procedimiento de compra identifica cuando el comprador accede a su página web ecommerce, elige cualquier cantidad de productos o servicios que desea comprar y selecciona el botón pagar.

2. El sistema del comercio realiza una serie de procesos internos como lo son: totalizar el valor de la compra incluyendo el iva, crear un registro dentro del sistema, crear una referencia interna y realizar el envió de los datos necesarios mediante el método POST

3. El comprador es redireccionado al checkout de Pago Seguro, en donde encontrara los datos anteriormente registrados y creados desde la plataforma del comercio, en este formulario de pagos deberá ingresar información correspondiente a su compra y los datos del pago.

4. Después de seleccionar el botón pagar Pago Seguro procesa la transacción con el canal transaccional que corresponda, obteniendo un resultado de dicha transacción (aprobado o negado) y mostrara el resultado en la página de respuesta.

5. Pago Seguro envía un correo de confirmación hacia el comprador y hacia el comercio el cual indica la referencia de la compra, el número de aprobación y el estado de la transacción.

6. Pago Seguro redirecciona al comprador a su página web ecommerce, esta pagina la establece el comercio como un url de respuesta.

# Observaciones

- Para realizar una integración mediante este servicio es necesario que cuente con personal de desarrollo el cual tenga conocimientos en lenguajes de programación, tales como:
  `PHP, Java, Python , .NET, Ruby, Javascript, etc.`

