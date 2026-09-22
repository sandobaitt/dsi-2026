# Descripción de Caso de Uso: CU-03 Crear Pedido

* **Actor:** Cliente
* **Precondición:** El cliente se encuentra autenticado en el sistema y cuenta con productos seleccionados en su carrito.
* **Postcondición:** El pedido queda registrado en el sistema en estado confirmado y el inventario queda actualizado.

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | Solicita concretar la compra de los productos de su carrito. |
| **2** | **Sistema** | Valida la disponibilidad de stock de los productos y solicita la dirección de entrega y forma de pago. |
| **3** | **Actor** | Ingresa la dirección de entrega, selecciona la forma de pago y confirma. |
| **4** | **Sistema** | Solicita a la pasarela de pagos externa la autorización del cobro por el monto total. |
| **5** | **Sistema** | Recibe la confirmación del pago aprobado. |
| **6** | **Sistema** | Registra el nuevo pedido, descuenta el stock de los productos vendidos y emite la factura. |
| **7** | **Sistema** | Informa el número de pedido confirmado y envía el comprobante. |

---

### Caminos Alternativos

#### 2.a Stock insuficiente de uno o más productos
1. El sistema informa los productos que no cuentan con existencias disponibles suficientes.
2. Fin del caso de uso.

#### 5.a Pago rechazado por la entidad financiera
1. El sistema informa que el cobro no fue autorizado por la pasarela de pagos.
2. Retoma en el paso 3.

#### 5.b Cancelación por parte del actor
1. El actor cancela la operación antes de la confirmación final.
2. Fin del caso de uso.
