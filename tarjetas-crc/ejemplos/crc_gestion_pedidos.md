# Ejemplo: Tarjetas CRC - Módulo de Gestión de Pedidos

Tarjetas CRC correspondientes al caso de uso **CU-03 Crear Pedido** y coherentes con el Diagrama de Clases y el Diagrama de Secuencia.

---

### Tarjeta 1: `Pedido`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `Pedido`
- **Propósito:** Representa la transacción comercial que formaliza la solicitud de compra realizada por un cliente, coordinando los ítems adquiridos, el cálculo del total monetario y su ciclo de vida de estados.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber:**<br>- Número único de pedido (`nroPedido`).<br>- Fecha y hora de creación (`fechaCreacion`).<br>- Estado de la transacción (`estado`: Pendiente, Aprobado, Cancelado).<br>- Monto total consolidado (`montoTotal`).<br>- Dirección de entrega asignada (`direccionEntrega`).<br><br>**Hacer:**<br>- Calcular el importe total sumando los subtotales de sus detalles.<br>- Confirmar la aprobación del pedido tras recibir confirmación de pago.<br>- Cancelar el pedido en caso de rechazo o timeout.<br>- Proveer su identificador para la facturación. | - **Cliente:** A quién pertenece la orden de compra.<br>- **DetallePedido:** A quien le solicita el cálculo del subtotal de cada ítem.<br>- **Factura:** A quien provee información para generar el comprobante fiscal.<br>- **Sistema / ControladorPedido:** Quien le solicita la creación y confirmación. |

---

### Tarjeta 2: `DetallePedido`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `DetallePedido`
- **Propósito:** Registrar la cantidad y el precio unitario histórico convenido de un producto específico dentro de un pedido determinado.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber:**<br>- Cantidad de unidades solicitadas (`cantidad`).<br>- Precio unitario histórico al momento de la compra (`precioUnitarioHistorico`).<br><br>**Hacer:**<br>- Calcular su propio subtotal (`cantidad * precioUnitarioHistorico`).<br>- Informar el producto al cual hace referencia. | - **Pedido:** Quien lo contiene y le solicita el subtotal.<br>- **Producto:** A quien consulta para conocer el producto asociado. |

---

### Tarjeta 3: `Producto`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `Producto`
- **Propósito:** Mantener la información del catálogo comercial, precio vigente y control de inventario/existencias disponibles para la venta.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber:**<br>- Identificador de producto (`idProducto`).<br>- Nombre y descripción comercial.<br>- Precio de venta actual (`precioActual`).<br>- Cantidad de unidades disponibles en stock (`stockActual`).<br><br>**Hacer:**<br>- Validar si cuenta con existencias suficientes para satisfacer una demanda (`tieneStockSuficiente(cantidad)`).<br>- Decrementar el inventario físico al confirmarse una venta (`decrementarStock(cantidad)`).<br>- Proveer el precio vigente para la cotización de pedidos. | - **Sistema:** Quien le solicita verificar existencias y decrementar stock.<br>- **DetallePedido:** Quien toma su precio al instanciarse. |

---

### Tarjeta 4: `Cliente`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `Cliente`
- **Propósito:** Representar al actor del negocio que efectúa transacciones de compra, manteniendo su perfil, historial y datos de contacto y entrega.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber:**<br>- Identificador de cliente (`idCliente`).<br>- Nombre completo y correo electrónico.<br>- Domicilio principal de entrega.<br><br>**Hacer:**<br>- Proveer los datos de contacto y facturación.<br>- Asociar los pedidos generados a su historial de compras. | - **Pedido:** Quien conoce al cliente titular de la orden.<br>- **Sistema:** Quien recupera al cliente a partir de la sesión. |

---

### Tarjeta 5: `ControladorPedido`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `ControladorPedido`
- **Propósito:** Orquestar el flujo de control del caso de uso *Crear Pedido*, mediando entre los eventos de la interfaz de usuario y las operaciones de la fachada del sistema.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber:**<br>- Estado actual del flujo de checkout.<br><br>**Hacer:**<br>- Recibir las acciones de la pantalla del carrito (`PantallaCarrito`).<br>- Consultar las credenciales activas al controlador de sesión (`CTRLSesion`).<br>- Solicitar la validación de inventario y registro de orden a `Sistema`.<br>- Devolver el resultado de la transacción a la vista. | - **PantallaCarrito:** Quien le delega los eventos del usuario.<br>- **CTRLSesion:** A quien le pide el usuario logueado.<br>- **Sistema:** A quien le solicita la ejecución de la lógica de negocio. |

---

### Tarjeta 6: `Sistema`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `Sistema`
- **Propósito:** Actuar como Fachada (*Facade*) y controlador central de operaciones de negocio del subsistema de ventas, administrando las colecciones de entidades y la coordinación de transacciones.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber:**<br>- Colección de productos disponibles en catálogo.<br>- Colección de pedidos registrados en el sistema.<br><br>**Hacer:**<br>- Buscar y verificar la disponibilidad de stock de productos.<br>- Crear e instanciar listas temporales de presentación.<br>- Instanciar y persistir una nueva orden `Pedido`.<br>- Coordinar el decremento de inventario con las instancias de `Producto`. | - **ControladorPedido:** Quien le solicita las operaciones del caso de uso.<br>- **Producto:** A quien consulta stock y solicita decrementar unidades.<br>- **Pedido:** A quien instancia y agrega a la colección de pedidos.<br>- **PasarelaPagosAdapter:** Con quien coordina la aprobación del cobro electrónico. |
