# Tarjetas CRC: Realización de CU Realizar Reserva (Reserva Paga)

> **Consigna:** Confeccionar 3 tarjetas CRC. Solo clases entidad, exceptuar la clase Sistema.

---

### Tarjeta 1: `Reserva`

#### 📇 Anverso (Frente)
* **Clase:** `Reserva`
* **Propósito:** Representar la transacción que formaliza la reserva y compra de entradas para un evento artístico por parte de un usuario, administrando su estado, importe total y asientos asignados.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Calcular el monto total sumando el valor de sus entradas asociadas.<br>- Confirmar la reserva tras la aprobación del pago.<br>- Proveer los detalles de la reserva para el calendario personal y comprobante.<br><br>**Saber:**<br>- Código identificador (`nroReserva`).<br>- Fecha y hora de emisión (`fechaHoraReserva`).<br>- Estado de la reserva (`estado`: Pendiente, Confirmada, Cancelada).<br>- Monto total liquidado (`montoTotal`). | - **Usuario:** A quien pertenece la reserva efectuada.<br>- **Evento:** Sobre el cual se realiza la reserva.<br>- **Entrada:** A quien contiene y solicita el importe unitario y asiento.<br>- **Sistema:** Quien instancia y administra la persistencia de la reserva. |

---

### Tarjeta 2: `Evento`

#### 📇 Anverso (Frente)
* **Clase:** `Evento`
* **Propósito:** Representar el espectáculo artístico (concierto, obra de teatro, exposición) ofrecido en la plataforma, controlando su información de cartelera, cupo y disponibilidad de entradas.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Verificar si cuenta con cupo o entradas disponibles para la cantidad requerida (`tieneDisponibilidad(cantidad)`).<br>- Decrementar el cupo de entradas disponibles al confirmarse una reserva (`decrementarCupo(cantidad)`).<br>- Proveer información del espectáculo (título, fecha, hora, lugar y precio).<br><br>**Saber:**<br>- Identificador (`idEvento`), título (`titulo`), tipo/género (`tipo`), fecha y hora (`fechaHora`), ubicación/sala (`ubicacion`), precio base de entrada (`precioEntrada`) y cupo disponible (`cupoDisponible`). | - **Sistema:** Quien consulta disponibilidad y solicita descontar cupo.<br>- **Reserva:** Quien referencia al evento reservado.<br>- **Entrada:** A quien provee el precio base para su emisión. |

---

### Tarjeta 3: `Entrada`

#### 📇 Anverso (Frente)
* **Clase:** `Entrada`
* **Propósito:** Representar la unidad individual de acceso y butaca/asiento reservada para un evento determinado dentro de una reserva.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Proveer el precio abonado y la ubicación/asiento asignado.<br>- Validar la identificación única de acceso.<br><br>**Saber:**<br>- Código de entrada (`codigoEntrada`), número de asiento/ubicación (`ubicacionAsiento`) y precio unitario (`precio`). | - **Reserva:** Quien la contiene en composición y le solicita sus datos.<br>- **Evento:** Espectáculo para el cual es válida la entrada. |
