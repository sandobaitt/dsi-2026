# Especificación de Caso de Uso: CU-03 Crear Pedido

## 1. Ficha del Caso de Uso

| Atributo | Detalle |
| :--- | :--- |
| **Identificador:** | `CU-03` |
| **Nombre del Caso de Uso:** | Crear Pedido |
| **Autor(es):** | Estudiante - DSI 2026 |
| **Versión / Fecha:** | `v1.0` - `22/09/2026` |
| **Módulo / Paquete:** | Ventas y Facturación |
| **Complejidad:** | Alta |

---

## 2. Propósito / Objetivo
Permitir a un cliente autenticado consolidar la compra de los productos acumulados en su carrito, registrar la orden de pedido, procesar el pago electrónico y disparar la reserva de stock.

---

## 3. Actores Involucrados
- **Actor Primario:** Cliente (inicia la compra).
- **Actor Secundario:** Pasarela de Pagos (procesa la transacción monetaria externa).

---

## 4. Precondiciones
1. El usuario debe estar autenticado en el sistema (`CTRLSesion` activa con credenciales de cliente).
2. El carrito de compras debe tener al menos un ítem con cantidad mayor a cero.
3. El catálogo y los ítems del carrito deben tener stock disponible.

---

## 5. Postcondiciones (Garantías)
- **Garantía de Éxito:** Se crea la entidad `Pedido` en estado "Confirmado", se descuenta el stock de las entidades `Producto`, se genera la `Factura` correspondiente y se limpia el carrito.
- **Garantía Mínima:** Si el pago es rechazado o el usuario cancela, no se cobra dinero, el carrito permanece inalterado y no se descuenta stock de inventario.

---

## 6. Disparador (Trigger)
El cliente presiona el botón "Finalizar Compra" desde la pantalla del carrito.

---

## 7. Flujo Principal de Eventos (Camino Feliz)

| Paso | Acción del Actor (Cliente) | Reacción / Proceso del Sistema |
| :---: | :--- | :--- |
| **1** | Presiona "Finalizar Compra" en la vista del carrito. | |
| **2** | | El sistema recupera el usuario logueado de `CTRLSesion`. |
| **3** | | El sistema valida la disponibilidad de stock de todos los ítems. |
| **4** | | El sistema solicita la dirección de entrega y los datos de pago. |
| **5** | Ingresa dirección de entrega y datos de tarjeta de crédito. | |
| **6** | Confirma la operación pulsando "Pagar Orden". | |
| **7** | | El sistema calcula el monto total (subtotal + envío). |
| **8** | | El sistema invoca al `CU-04: Procesar Pago` comunicándose con la Pasarela de Pagos. |
| **9** | | El sistema recibe la confirmación de transacción aprobada. |
| **10**| | El sistema registra la nueva instancia de `Pedido` asociada al Cliente. |
| **11**| | El sistema descuenta las existencias de cada `Producto` en el inventario. |
| **12**| | El sistema emite la `Factura` y muestra la pantalla de confirmación con el número de pedido. |

---

## 8. Flujos Alternativos y Excepcionales

### Flujo Alternativo 3a: Stock Insuficiente en uno o más ítems
- **Condición:** Al verificar existencias, uno de los productos no cuenta con el stock pedido.
- **Acciones:**
  1. El sistema informa al cliente los productos que no poseen disponibilidad suficiente.
  2. El cliente actualiza la cantidad o retira los productos en conflicto.
  3. El flujo regresa al **Paso 1**.

### Flujo Alternativo 5a: Aplicar Cupón Promocional (Punto de Extensión PE-01)
- **Condición:** El cliente ingresa un cupón de descuento válido.
- **Acciones:**
  1. El sistema invoca al `CU-05: Aplicar Cupón de Descuento`.
  2. El sistema recalcula el total bonificado.
  3. El flujo continúa en el **Paso 6**.

### Flujo Excepcional 9a: Rechazo de Tarjeta / Fondos Insuficientes
- **Condición:** La Pasarela de Pagos responde con transacción rechazada.
- **Acciones:**
  1. El sistema muestra mensaje de error informando el motivo del rechazo bancario.
  2. El sistema no crea el pedido ni descuenta inventario (Garantía Mínima).
  3. El sistema permite al cliente seleccionar otro método de pago (regresa al **Paso 5**) o cancelar la operación.

---

## 9. Puntos de Extensión
- **PE-01 (Aplicar Cupón de Descuento):** Disponible en el Paso 5 antes de pulsar "Pagar Orden".

---

## 10. Reglas de Negocio Asociadas
- **RN-01:** Monto mínimo de compra para acceder a envío gratuito: $15.000.
- **RN-02:** Tiempo máximo de reserva de stock durante el checkout: 15 minutos.
