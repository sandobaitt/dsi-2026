# Ejemplo: Tarjetas CRC - Realización de CU-03 Crear Pedido

Tarjetas CRC correspondientes a la totalidad de las clases participantes en el Diagrama de Clases y Diagrama de Secuencia del **CU-03 Crear Pedido**.

---

### Tarjeta 1: `PantallaCarrito` (UI)

#### 📇 Anverso (Frente)
* **Clase:** `PantallaCarrito`
* **Propósito:** Interfaz de usuario responsable de capturar los eventos del cliente y desplegar la información, confirmaciones y mensajes del caso de uso.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Solicitar y capturar datos de entrega y pago.<br>- Mostrar confirmación exitosa con número de pedido.<br>- Mostrar mensaje de error si el pago es rechazado o el stock es insuficiente.<br><br>**Saber:**<br>- *No posee atributos de estado (Regla de Cátedra).* | - **ControladorPedido:** A quien le delega los eventos iniciados por el cliente. |

---

### Tarjeta 2: `ControladorPedido` (CTRL)

#### 📇 Anverso (Frente)
* **Clase:** `ControladorPedido`
* **Propósito:** Coordinar el flujo de ejecución del caso de uso Crear Pedido entre la interfaz, la sesión activa y el sistema.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Iniciar la creación del pedido.<br>- Consultar el usuario autenticado al controlador de sesión.<br>- Solicitar validación y cobro a la entidad bancaria.<br>- Solicitar a Sistema la validación de inventario y el registro del pedido.<br>- Informar el resultado a la pantalla.<br><br>**Saber:**<br>- *No posee atributos de estado (Regla de Cátedra).* | - **PantallaCarrito:** A quien responde con el resultado del flujo.<br>- **ControladorSesion:** A quien consulta las credenciales del usuario activo.<br>- **Sistema:** A quien delega las operaciones de negocio y persistencia.<br>- **Entidad Bancaria (Actor Externo):** A quien solicita validar y autorizar el pago con tarjeta. |

---

### Tarjeta 3: `ControladorSesion` (CTRL)

#### 📇 Anverso (Frente)
* **Clase:** `ControladorSesion`
* **Propósito:** Proveer el contexto de autenticación y datos del usuario activo para evitar solicitar información ya conocida por el sistema.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Proveer la instancia del usuario o cliente autenticado.<br>- Proveer el identificador o legajo de la sesión.<br><br>**Saber:**<br>- *No posee atributos de estado en la modelación de la realización (Regla de Cátedra).* | - **ControladorPedido:** Quien le solicita los datos del usuario logueado. |

---

### Tarjeta 4: `Sistema` (Fachada)

#### 📇 Anverso (Frente)
* **Clase:** `Sistema`
* **Propósito:** Actuar como Fachada global y coordinador de negocio; administra las colecciones principales y realiza búsquedas y altas de transacciones.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Obtener y verificar stock de los productos del carrito.<br>- Crear e instanciar listas temporales de presentación.<br>- Registrar y persistir la nueva instancia de Pedido.<br>- Decrementar el stock físico de los productos vendidos.<br><br>**Saber:**<br>- Colección global de productos en catálogo.<br>- Colección global de pedidos registrados. | - **ControladorPedido:** Quien le solicita las operaciones del CU.<br>- **Producto:** A quien consulta stock y solicita decrementar inventario.<br>- **Pedido:** A quien instancia y agrega a su colección.<br>- **Cliente:** A quien asocia con el nuevo pedido. |

---

### Tarjeta 5: `Pedido` (Dominio - Transacción)

#### 📇 Anverso (Frente)
* **Clase:** `Pedido`
* **Propósito:** Entidad transaccional central que relaciona al cliente con los productos adquiridos, formalizando la orden de compra y su importe total.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Calcular el importe total sumando los subtotales de sus detalles.<br>- Confirmar la aprobación de la orden.<br>- Proveer su número identificador.<br><br>**Saber:**<br>- Número de pedido (`nroPedido`), fecha de creación (`fechaCreacion`), estado (`estado`), monto total (`montoTotal`) y dirección de entrega (`direccionEntrega`). | - **Cliente:** A quien pertenece el pedido.<br>- **DetallePedido:** A quien le solicita el subtotal de cada ítem.<br>- **Factura:** A quien provee información para emitir el comprobante.<br>- **Sistema:** Quien lo crea y lo administra. |

---

### Tarjeta 6: `DetallePedido` (Dominio)

#### 📇 Anverso (Frente)
* **Clase:** `DetallePedido`
* **Propósito:** Registrar la línea de ítem de un pedido, congelando la cantidad solicitada y el precio unitario histórico de venta.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Calcular su subtotal multiplicando cantidad por precio unitario.<br>- Informar el producto referenciado.<br><br>**Saber:**<br>- Cantidad de unidades (`cantidad`) y precio unitario histórico (`precioUnitarioHistorico`). | - **Pedido:** Quien lo contiene en composición.<br>- **Producto:** A quien referencia para obtener el precio de catálogo al crearse. |

---

### Tarjeta 7: `Producto` (Dominio)

#### 📇 Anverso (Frente)
* **Clase:** `Producto`
* **Propósito:** Entidad del catálogo que gestiona la información comercial del artículo, precio vigente y control de inventario.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Validar si cuenta con existencias suficientes (`tieneStockSuficiente(cantidad)`).<br>- Reducir el stock disponible al confirmarse la compra (`decrementarStock(cantidad)`).<br>- Proveer precio actual y existencias.<br><br>**Saber:**<br>- Identificador (`idProducto`), nombre (`nombre`), precio (`precioActual`) y existencias (`stockActual`). | - **Sistema:** Quien le consulta disponibilidad y ordena descontar existencias.<br>- **DetallePedido:** Quien toma su precio para la línea de pedido. |

---

### Tarjeta 8: `Cliente` (Dominio)

#### 📇 Anverso (Frente)
* **Clase:** `Cliente`
* **Propósito:** Representar al comprador en el dominio del problema, manteniendo sus datos personales y domicilio.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Proveer sus datos de contacto y dirección de entrega.<br><br>**Saber:**<br>- Identificador (`idCliente`), nombre completo (`nombre`), correo (`email`) y dirección (`direccion`). | - **Pedido:** Quien mantiene la asociación con el cliente titular.<br>- **Sistema:** Quien lo vincula al pedido a partir de la sesión. |

---

### Tarjeta 9: `Factura` (Dominio)

#### 📇 Anverso (Frente)
* **Clase:** `Factura`
* **Propósito:** Comprobante fiscal generado a partir de una orden de pedido confirmada.

#### 🔄 Reverso (Dorso)
| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Emitir el comprobante con los importes finales del pedido.<br><br>**Saber:**<br>- Número de comprobante (`nroComprobante`), fecha de emisión (`fechaEmision`) e importe total (`importeTotal`). | - **Pedido:** A partir de quien se obtienen los datos de facturación. |
