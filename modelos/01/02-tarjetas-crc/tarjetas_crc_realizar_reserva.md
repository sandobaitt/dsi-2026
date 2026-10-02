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
| **Hacer:**<br>- Crear y agregar una entrada por cada asiento reservado (`agregarEntrada(asiento)`).<br>- Confirmar la reserva tras la aprobación del pago, actualizando su propio estado (`confirmar()`).<br><br>**Saber:**<br>- Código identificador (`nroReserva`).<br>- Fecha y hora de emisión (`fechaHoraReserva`).<br>- Estado de la reserva (`estado`: Pendiente, Confirmada, Cancelada).<br>- Monto total liquidado (`montoTotal`). | - **Usuario:** A quien pertenece la reserva efectuada.<br>- **Evento:** Sobre el cual se realiza la reserva.<br>- **Entrada:** A quien crea y contiene en composición.<br>- **Sistema:** Quien instancia y administra la persistencia de la reserva. |

---

### Tarjeta 2: `Evento`

#### 📇 Anverso (Frente)
* **Clase:** `Evento`
* **Propósito:** Representar el espectáculo artístico (concierto, obra de teatro, exposición) ofrecido en la plataforma, controlando su información de cartelera, cupo y disponibilidad de entradas.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Verificar si cuenta con cupo o entradas disponibles para la cantidad requerida (`tieneDisponibilidad(cantidad)`).<br>- Calcular el importe de la cantidad de entradas solicitadas a partir de su precio (`calcularImporte(cantidad)`).<br>- Decrementar el cupo de entradas disponibles al confirmarse una reserva (`decrementarCupo(cantidad)`).<br><br>**Saber:**<br>- Identificador (`idEvento`), título (`titulo`), tipo/género (`tipo`), fecha y hora (`fechaHora`), ubicación/sala (`ubicacion`), precio base de entrada (`precioEntrada`) y cupo disponible (`cupoDisponible`). | - **Sistema:** Quien consulta disponibilidad, solicita el importe y descontar cupo.<br>- **Reserva:** Quien referencia al evento reservado. |

---

### Tarjeta 3: `Entrada`

#### 📇 Anverso (Frente)
* **Clase:** `Entrada`
* **Propósito:** Representar la unidad individual de acceso y butaca/asiento reservada para un evento determinado dentro de una reserva.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Registrar el asiento asignado al ser creada por la reserva (`create(asiento)`).<br><br>**Saber:**<br>- Código de entrada (`codigoEntrada`) y número de asiento/ubicación (`ubicacionAsiento`). | - **Reserva:** Quien la crea y la contiene en composición. |
