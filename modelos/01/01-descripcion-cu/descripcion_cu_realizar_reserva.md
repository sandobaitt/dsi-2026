# Descripción de Caso de Uso: Realizar Reserva (Reserva Paga)

* **Actor:** Usuario Registrado
* **Precondición:** El usuario se encuentra autenticado en el sistema y ha seleccionado un evento pago con fecha y ubicación definidas.
* **Postcondición:** La reserva queda registrada en el sistema con estado confirmada, las entradas disponibles del evento quedan descontadas y el evento se incorpora al calendario personal del usuario.

---

### Camino Estándar (Reserva Paga)

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | Solicita realizar la reserva del evento seleccionado, indicando la cantidad de entradas y ubicación/asientos deseados. |
| **2** | **Sistema** | Verifica la disponibilidad de entradas para el evento, calcula el importe total y solicita seleccionar el medio de pago. |
| **3** | **Actor** | Selecciona el medio de pago (billetera electrónica, tarjeta de crédito o débito), ingresa los datos de pago y confirma la operación. |
| **4** | **Sistema** | Solicita a la entidad financiera externa la autorización y cobro del importe correspondiente. |
| **5** | **Sistema** | Recibe la confirmación del pago aprobado por parte de la entidad externa. |
| **6** | **Sistema** | Registra la reserva en estado confirmada, descuenta las entradas del evento, genera los comprobantes asignados y actualiza el calendario personal del usuario. |
| **7** | **Sistema** | Informa el código de confirmación de la reserva y los detalles de asistencia. |

---

### Caminos Alternativos

#### 2.a Entradas o cupo no disponible
1. El sistema informa que la cantidad de entradas solicitadas excede la disponibilidad actual del evento.
2. Fin del caso de uso.

#### 5.a Medio de pago rechazado
1. El sistema informa que la transacción no fue autorizada por la entidad financiera correspondiente.
2. Retoma en el paso 3.
