# Tarjetas CRC: Realización de CU Reasignar Tickets por Ausencia de Desarrollador

> **Pautas de Cátedra DSI 2026:** Formato oficial con Anverso (Clase, Propósito) y Reverso (Responsabilidades: Hacer / Saber, Colaboración) enfocado estrictamente en las clases del caso de uso analizado.

---

### Tarjeta 1: `Ticket`

#### 📇 Anverso (Frente)
* **Clase:** `Ticket`
* **Propósito:** Representar la unidad de trabajo asignada a un desarrollador en un proyecto de software, administrando su estado de avance, prioridad, referencia a Trello y su asignación activa.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Verificar si se encuentra en estado activo (`estaActivo()`: abierto o en progreso).<br>- Reasignar su desarrollador responsable (`reasignarDesarrollador(nuevoDev)`).<br>- Proveer el identificador o enlace de tarjeta de Trello asociada (`getIdTarjetaTrello()`).<br>- Proveer detalles descriptivos de la tarea para la lista de candidatos.<br><br>**Saber:**<br>- `idTicket`, `descripcion`, `prioridad`, `estado` (Abierto, En progreso, Resuelto, Cancelado, Cerrado).<br>- `idTarjetaTrello` (referencia externa).<br>- `desarrolladorAsignado` (`Desarrollador`). | - **Desarrollador:** A quien se encuentra asignada la tarea.<br>- **Reasignacion:** A quien referencia como objeto modificado.<br>- **Sistema:** Quien consulta sus datos y solicita el cambio de responsable. |

---

### Tarjeta 2: `Reasignacion`

#### 📇 Anverso (Frente)
* **Clase:** `Reasignacion`
* **Propósito:** Registrar de forma inmutable la trazabilidad histórica de cada transferencia de ticket efectuada por un Líder Técnico ante la ausencia de un integrante del equipo.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Registrar los datos de la transferencia (ticket, fecha, hora, responsable y desarrolladores intervinientes).<br>- Proveer el detalle consolidado para el resumen final de operaciones.<br><br>**Saber:**<br>- `idReasignacion`.<br>- `fechaHora` (momento de la operación).<br>- `motivo` ("Ausencia de desarrollador").<br>- `liderResponsable` (`LiderTecnico`).<br>- `desarrolladorAnterior` (`Desarrollador`).<br>- `desarrolladorNuevo` (`Desarrollador`).<br>- `ticket` (`Ticket`). | - **LiderTecnico:** Quien ejecuta y responde por la reasignación.<br>- **Desarrollador:** Integrante cedente e integrante receptor.<br>- **Ticket:** Unidad de trabajo reasignada.<br>- **Sistema:** Quien la instancia tras cada reasignación confirmada. |

---

### Tarjeta 3: `Desarrollador`

#### 📇 Anverso (Frente)
* **Clase:** `Desarrollador`
* **Propósito:** Representar a los profesionales de desarrollo que integran los equipos de NubeSoft, conociendo su información personal y permitiendo evaluar su carga de trabajo en el sprint.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Proveer su identificación y nombre completo.<br>- Calcular su carga de trabajo actual dentro del sprint vigente (`calcularCargaSprint()`).<br>- Proveer su estado de disponibilidad (activo, licencia, vacaciones).<br><br>**Saber:**<br>- `idDesarrollador`, `nombre`, `apellido`, `email`, `estadoDisponibilidad`. | - **Ticket:** Tareas activas que tiene asignadas en el sprint.<br>- **Equipo:** Estructura a la que pertenece operativamente.<br>- **Sistema:** Quien solicita su información y balance de carga. |

---

### Tarjeta 4: `Equipo`

#### 📇 Anverso (Frente)
* **Clase:** `Equipo`
* **Propósito:** Agrupar a los recursos humanos (Líder Técnico y Desarrolladores) asignados a los proyectos de NubeSoft y coordinar la pertenencia en el sprint activo.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Proveer la nómina de desarrolladores que componen el equipo (`getDesarrolladores()`).<br>- Proveer el sprint vigente para el cálculo de carga laboral (`getSprintVigente()`).<br><br>**Saber:**<br>- `idEquipo`, `nombreEquipo`, `liderTecnico` (`LiderTecnico`), lista de desarrolladores (`Desarrollador[*]`). | - **LiderTecnico:** Responsable de la coordinación del equipo.<br>- **Desarrollador:** Miembros subordinados del equipo.<br>- **Sistema:** Quien consulta la composición del equipo del Líder activo. |
