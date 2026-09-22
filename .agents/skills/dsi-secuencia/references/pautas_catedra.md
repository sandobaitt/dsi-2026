# Pautas de Cátedra y Puntos de Consulta Docente (DSI)

Este documento profundiza en las justificaciones de cada pauta indicada por la cátedra y detalla las inconsistencias y consultas recomendadas para realizar al docente o ayudante de trabajos prácticos.

---

## 🔍 Desglose Técnico de las Pautas de la Cátedra

### 1. Control de Sesión (`CTRLSesion`)
- **Propósito:** Separación de responsabilidades. La autenticación es un aspecto transversal. Una vez que el usuario ingresó al sistema, la sesión activa almacena su contexto (`legajo`, `idUsuario`, `rol`, etc.).
- **Regla:** En los diagramas de secuencia de casos de uso operativos (ej: *Registrar Venta*, *Inscribir a Examen*), el actor humano nunca debe ingresar su propio legajo o datos que el sistema ya conoce. El `CTRLCU` debe delegar esa consulta al `CTRLSesion`.
- **Notación:** Se representa como una línea de vida preexistente (sin mensaje `new` / `create`).

### 2. Nomenclatura `obtener...()` vs `get...()`
- **Capa Control / Negocio (`CTRL` $\rightarrow$ `Sistema`):**
  - Los mensajes entre controladores y la fachada del sistema representan operaciones de alto nivel del caso de uso. Deben expresar intención de negocio: `obtenerAlumnosInscriptos()`, `buscarTurnosDisponibles()`, `calcularMontoTotal()`.
- **Capa de Dominio / Entidades (`Sistema` $\rightarrow$ `Entidad`):**
  - Los mensajes entre el sistema y las instancias de entidad representan consultas directas de atributos de estado (propiedades encapsuladas). Por estándar de Programación Orientada a Objetos, se utiliza `getNombre()`, `getLegajo()`, `getPrecio()`.

### 3. Creación y Ciclo de Vida de Listas Temporales
- **Creador (Patrón GRASP Creator):** El objeto `Sistema` contiene o coordina la búsqueda, por lo tanto es quien crea la lista temporal (ej: `create` sobre `listaDTO` o `listaTemp`).
- **Retorno implícito de referencia:** Al hacer `Sistema -> listaTemp: add(item)`, `Sistema` ya tiene el puntero a la colección en su memoria local. Modelar una flecha `listaTemp --> Sistema` es redundante e incorrecto bajo la semántica UML.
- **Omisión de `destroy`:** En la especificación formal UML 2.x, un objeto efímero finaliza con una cruz (`destroy`). Sin embargo, en exámenes presenciales o parciales esto satura el espacio gráfico sin aportar valor al diseño lógico. La cátedra establece formalmente que omitirlo **no descuenta puntaje**.

### 4. Retornos sin palabra clave `return`
- En PlantUML y UML estándar, la flecha discontinua (`-->`) define intrínsecamente un mensaje de respuesta (`reply message`).
- Escribir `return: x` o `retornar(x)` genera ruido visual. Basta con rotular la flecha con el nombre del dato o variable devuelta (`--> CTRLCU: listaFiltrada`).

---

## ❓ Puntos de Consulta / Inconsistencias a Confirmar con el Docente

A continuación se listan las consultas puntuales para trasladar a la profesora o ayudantes para asegurar máxima compatibilidad con su criterio de corrección:

### Consulta 1: "Una clase que relacione las 3 clases del sistema"
- **Contexto:** En el tip se indicó: *"El diagrama de clases habría que tener una clase que relacione las 3 clases del sistema"*.
- **Pregunta concreta para el docente:**
  > *"Profe, cuando en el enunciado tenemos tres clases principales del sistema (ej: Cliente, Producto, Vendedor), ¿la clase que las relaciona se refiere a una **clase asociativa / transacción** del modelo de dominio (como `Pedido`, `Factura` o `Reserva`), o se refiere a la clase **`Sistema`** actuando como Fachada/Controlador de negocio que conoce las colecciones de las tres entidades?"*
- **Impacto en el parcial:** Si se refiere al dominio, no debe faltar la entidad transaccional intermedia que resuelve las relaciones N a N. Si se refiere a la arquitectura, la clase `Sistema` debe tener los enlaces (`1 -> *`) hacia los repositorios/colecciones.

### Consulta 2: Destinatario directo del retorno de la lista temporal
- **Contexto:** El tip menciona: *"El retorno de la lista completa debe graficarse desde el Sistema hacia el Controlador o la UI"*.
- **Pregunta concreta para el docente:**
  > *"¿En la cátedra se permite que el mensaje de retorno de la lista vaya directamente desde `Sistema` a la `Pantalla/UI`, o el retorno debe viajar obligatoriamente `Sistema --> CTRLCU` y luego el `CTRLCU` envía los datos a la `Pantalla/UI` para respetar el patrón Controlador?"*
- **Recomendación por defecto:** Mantener el desacoplamiento estricto (`Sistema --> CTRLCU --> Pantalla`). Es la opción universalmente más segura en cualquier corrección académica.

### Consulta 3: Creación de nuevas entidades de negocio
- **Contexto:** La cátedra estableció que `Sistema` es el encargado exclusivo de crear las listas temporales.
- **Pregunta concreta para el docente:**
  > *"Cuando en el caso de uso se crea una entidad definitiva del dominio (por ejemplo, instanciar un nuevo `Pedido` o una nueva `Inscripcion`), ¿quién debe enviarle el mensaje `create` / `new`: el `CTRLCU` o la clase `Sistema`?"*
- **Recomendación por defecto:** En la mayoría de las variantes de DSI basadas en Larman/GRASP, el `CTRLCU` delega la operación en `Sistema: registrarPedido(...)` y es `Sistema` quien instancia el nuevo `Pedido` y lo agrega a su colección para asegurar persistencia y consistencia.
