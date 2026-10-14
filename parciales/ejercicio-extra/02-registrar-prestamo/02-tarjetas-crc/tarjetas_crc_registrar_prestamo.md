# Tarjetas CRC: Realización de CU Registrar Préstamo

> Una tarjeta por cada clase del diagrama de clases del CU (UI, Controlador, Sistema y clases de dominio). UI y Controlador **no llevan atributos**.

---

### Tarjeta 1: `PantallaRegistrarPrestamo`

#### 📇 Anverso (Frente)
* **Clase:** `PantallaRegistrarPrestamo`
* **Propósito:** Interactuar con el bibliotecario durante el registro de un préstamo: recibir sus acciones y datos, y mostrarle información, resultados y errores, sin intervenir en las reglas del negocio.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Recibir la solicitud de registrar un préstamo (`seleccionarOpcionRegistrarPrestamo()`).<br>- Pedir el legajo del estudiante (`solicitarLegajo()`) y recibirlo (`ingresarLegajo(nroLegajo)`).<br>- Pedir el tipo de préstamo y los ejemplares (`solicitarTipoYEjemplares()`) y recibirlos (`ingresarTipoYEjemplares(idTipoPrestamo, codigos)`).<br>- Mostrar el resumen del préstamo (`mostrarResumenPrestamo(ejemplares)`) y recibir la confirmación (`confirmarPrestamo()`).<br>- Mostrar el préstamo registrado con sus fechas límite (`mostrarPrestamoRegistrado(p)`).<br>- Mostrar los errores de los caminos alternativos (`mostrarError(mensaje)`).<br><br>**Saber:**<br>- No conoce datos del negocio (no lleva atributos). | - **Bibliotecario (actor):** Quien opera la pantalla.<br>- **CTRLRegistrarPrestamo:** A quien le comunica las acciones del actor y de quien recibe qué mostrar. |

---

### Tarjeta 2: `CTRLRegistrarPrestamo`

#### 📇 Anverso (Frente)
* **Clase:** `CTRLRegistrarPrestamo`
* **Propósito:** Coordinar el flujo del caso de uso Registrar Préstamo, comunicando a la pantalla con el Sistema.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Iniciar el caso de uso y pedir el legajo (`iniciarRegistroPrestamo()`).<br>- Recibir el legajo, buscar al estudiante, validar su condición de regular (CU incluido) y verificar su habilitación (`tomarLegajo(nroLegajo)`).<br>- Recibir el tipo de préstamo y los ejemplares, y solicitar su verificación (`tomarTipoYEjemplares(idTipoPrestamo, codigos)`).<br>- Recibir la confirmación y solicitar el registro del préstamo (`tomarConfirmacion()`).<br>- Indicar a la pantalla qué mostrar y destruirla al finalizar.<br><br>**Saber:**<br>- No conoce datos del negocio (no lleva atributos). | - **PantallaRegistrarPrestamo:** Recibe sus acciones y le indica qué mostrar.<br>- **Sistema:** A quien le delega verificar y registrar. |

---

### Tarjeta 3: `Sistema`

#### 📇 Anverso (Frente)
* **Clase:** `Sistema`
* **Propósito:** Fachada de negocio de BiblioUTN: conoce las colecciones de estudiantes, ejemplares y tipos de préstamo, y realiza las verificaciones y altas que requiere el registro de un préstamo.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Buscar al estudiante por legajo (`buscarEstudiante(nroLegajo)`).<br>- Determinar si el estudiante es regular consultando a Alumnado; es el CU incluido Validar Condición Regular (`validarCondicionRegular(nroLegajo)`).<br>- Verificar que el estudiante no tenga libros vencidos ni sanción vigente (`verificarHabilitacion(e)`).<br>- Verificar el cupo del estudiante, buscar cada ejemplar por código, controlar que esté disponible y armar la lista temporal de ejemplares (`verificarEjemplares(e, codigos)`).<br>- Calcular la fecha límite según el tipo de préstamo, crear el préstamo con sus detalles, pasar los ejemplares a prestados y asociar el préstamo al estudiante (`registrarPrestamo(e, idTipoPrestamo, ejemplares)`).<br><br>**Saber:**<br>- Colección de estudiantes, colección de ejemplares y colección de tipos de préstamo. | - **CTRLRegistrarPrestamo:** Quien le solicita las operaciones.<br>- **Estudiante, Ejemplar, TipoPrestamo, Prestamo:** A quienes consulta o instancia.<br>- **Sistema de Alumnado (actor externo):** A quien le consulta si el estudiante es regular, dentro del CU incluido. |

---

### Tarjeta 4: `Estudiante`

#### 📇 Anverso (Frente)
* **Clase:** `Estudiante`
* **Propósito:** Representar al estudiante que solicita préstamos, conociendo sus préstamos y sanciones para determinar si está habilitado para llevar nuevos ejemplares.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Determinar si tiene libros vencidos sin devolver (`tieneLibrosVencidos(fechaActual)`).<br>- Determinar si tiene una sanción vigente (`tieneSancionVigente(fechaActual)`).<br>- Determinar si puede llevar una cantidad de ejemplares sin superar el máximo de tres prestados a la vez (`puedeLlevar(cantidad)`).<br>- Incorporar un nuevo préstamo a su historial (`agregarPrestamo(p)`).<br><br>**Saber:**<br>- `nroLegajo`, `nombre`, `apellido`, el límite de ejemplares (`LIMITE_EJEMPLARES` = 3), sus préstamos y sus sanciones. | - **Sistema:** Quien le consulta su situación.<br>- **Prestamo:** A quien le pregunta si tiene ejemplares vencidos y cuántos pendientes.<br>- **Sancion:** A quien le pregunta si está vigente. |

---

### Tarjeta 5: `Sancion`

#### 📇 Anverso (Frente)
* **Clase:** `Sancion`
* **Propósito:** Representar una sanción del historial de un estudiante, que lo inhabilita para pedir préstamos durante un período.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Determinar si está vigente en una fecha dada (`estaVigente(fechaActual)`).<br><br>**Saber:**<br>- `motivo`, `fechaInicio` y `fechaFin`. | - **Estudiante:** Quien la contiene en su historial y le consulta su vigencia. |

---

### Tarjeta 6: `Prestamo`

#### 📇 Anverso (Frente)
* **Clase:** `Prestamo`
* **Propósito:** Representar el préstamo de uno o más ejemplares a un estudiante, administrando los detalles que lo componen.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Agregar un detalle por cada ejemplar prestado con su fecha límite, creándolo (`agregarDetalle(ejemplar, fechaLimite)`).<br>- Determinar si alguno de sus detalles está vencido (`tieneVencidos(fechaActual)`).<br>- Contar cuántos de sus ejemplares siguen sin devolverse (`cantidadPendientes()`).<br><br>**Saber:**<br>- `nroPrestamo`, `fechaPrestamo`, su estudiante, su tipo de préstamo y sus detalles. | - **Sistema:** Quien la instancia y le agrega los detalles.<br>- **Estudiante:** A quien pertenece y quien le consulta su estado.<br>- **TipoPrestamo:** Tipo con el que se prestó.<br>- **DetallePrestamo:** A quienes crea y consulta. |

---

### Tarjeta 7: `DetallePrestamo`

#### 📇 Anverso (Frente)
* **Clase:** `DetallePrestamo`
* **Propósito:** Representar el ejemplar prestado dentro de un préstamo, con su fecha límite y su fecha de devolución.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Determinar si el ejemplar sigue sin devolverse (`estaPendiente()`).<br>- Determinar si está vencido: sin devolver y con la fecha límite superada (`estaVencido(fechaActual)`).<br><br>**Saber:**<br>- `fechaLimite`, `fechaDevolucion` (vacía mientras no se devuelve) y su ejemplar. | - **Prestamo:** Quien lo crea y lo consulta.<br>- **Ejemplar:** Ejemplar físico prestado. |

---

### Tarjeta 8: `TipoPrestamo`

#### 📇 Anverso (Frente)
* **Clase:** `TipoPrestamo`
* **Propósito:** Representar la modalidad del préstamo (a domicilio o en sala) y el plazo de devolución que le corresponde.

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Calcular la fecha límite de devolución a partir de la fecha del préstamo (`calcularFechaLimite(fechaPrestamo)`).<br><br>**Saber:**<br>- `idTipoPrestamo`, `nombre` y `diasPlazo` (7 a domicilio, 0 en sala). | - **Sistema:** Quien lo busca y le pide la fecha límite.<br>- **Prestamo:** Quien lo referencia. |

---

### Tarjeta 9: `Ejemplar`

#### 📇 Anverso (Frente)
* **Clase:** `Ejemplar`
* **Propósito:** Representar cada copia física de un libro, identificada por su código de inventario, controlando su estado (disponible, prestado, en reparación o dado de baja).

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer:**<br>- Determinar si está disponible para prestarse (`estaDisponible()`).<br>- Pasar a estado prestado cuando se registra el préstamo (`prestar()`).<br><br>**Saber:**<br>- `codigoInventario` y `estado`. | - **Sistema:** Quien lo busca por código y le solicita su disponibilidad y su cambio de estado.<br>- **DetallePrestamo:** Quien lo referencia. |
