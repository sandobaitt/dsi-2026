# Especificación de Caso de Uso: CU-03 Crear Pedido

## Información General

| Campo | Detalle |
| :--- | :--- |
| **Identificador:** | `CU-03` |
| **Nombre:** | Crear Pedido |
| **Autor:** | Estudiante - DSI 2026 |
| **Versión:** | `1.0` |
| **Complejidad:** | Alta |

---

## 1. Descripción
Permite a un cliente registrado seleccionar uno o más productos del catálogo, indicar los datos de entrega, seleccionar el método de pago y consolidar la orden de compra en el sistema.

---

## 2. Actores
- **Actor Primario:** Cliente (inicia la compra).
- **Actor Secundario:** Pasarela de Pagos (procesa la transacción monetaria).

---

## 3. Precondiciones
1. El cliente debe estar autenticado en el sistema (`CU-01: Iniciar Sesión`).
2. El cliente debe tener al menos un producto seleccionado en su carrito con cantidad > 0.
3. Los productos seleccionados deben tener stock disponible en almacén.

---

## 4. Postcondiciones
- **Garantía de Éxito:** Se crea la orden con estado "Pendiente de Despacho", el stock queda reservado/descontado, se genera el comprobante y se notifica al cliente por correo.
- **Garantía Mínima:** Si el pago es rechazado o el usuario cancela, no se cobra dinero al cliente y el carrito permanece intacto sin reservar stock permanentemente.

---

## 5. Disparador
El cliente pulsa el botón "Finalizar Compra" desde la vista del carrito.

---

## 6. Flujo Principal de Eventos

| Paso | Acción del Actor (Cliente) | Reacción / Proceso del Sistema |
| :---: | :--- | :--- |
| **1** | Solicita finalizar la compra desde el carrito. | |
| **2** | | Valida disponibilidad de stock para los ítems seleccionados. |
| **3** | | Solicita dirección de entrega y método de pago. |
| **4** | Selecciona dirección guardada y método de pago (Tarjeta). | |
| **5** | | Calcula total de la orden (subtotal + costo de envío - descuentos). |
| **6** | Confirma la orden pulsando "Pagar Orden". | |
| **7** | | Invoca a `CU-04: Procesar Pago` comunicándose con la Pasarela de Pagos. |
| **8** | | Recibe confirmación de cobro exitoso de la Pasarela. |
| **9** | | Registra la orden de compra con estado "Aprobado". |
| **10**| | Descuenta el stock físico de los productos. |
| **11**| | Muestra pantalla de éxito con número de orden y envía correo de confirmación. |

---

## 7. Flujos Alternativos y Excepcionales

### Flujo Alternativo 2a: Stock Insuficiente
- **2a.1:** El sistema detecta que uno de los ítems ya no tiene stock suficiente.
- **2a.2:** El sistema notifica al cliente indicando el producto y la cantidad disponible máxima.
- **2a.3:** El cliente ajusta la cantidad o retira el producto del carrito.
- **2a.4:** El flujo vuelve al **Paso 1**.

### Flujo Alternativo 4a: Aplicación de Cupón de Descuento (Punto de Extensión PE-01)
- **4a.1:** El cliente introduce un código de cupón y pulsa "Aplicar Cupón".
- **4a.2:** Se ejecuta `CU-05: Aplicar Cupón de Descuento`.
- **4a.3:** El sistema actualiza el monto total bonificado.
- **4a.4:** El flujo continúa en el **Paso 5**.

### Flujo Excepcional 8a: Pago Rechazado por la Pasarela
- **8a.1:** La Pasarela de Pagos responde con código de error (fondos insuficientes o tarjeta rechazada).
- **8a.2:** El sistema informa al cliente la causa del rechazo.
- **8a.3:** El sistema ofrece ingresar un nuevo método de pago o reintentar.
- **8a.4:** Si el cliente reintenta, vuelve al **Paso 4**. Si cancela, el flujo termina sin registrar orden ni descontar stock.

---

## 8. Puntos de Extensión
- **PE-01 (Aplicar Cupón de Descuento):** Disponible en el Paso 4 antes del cálculo final del total.
