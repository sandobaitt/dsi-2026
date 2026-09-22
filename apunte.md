# Apunte Parcial Práctico 2026

A partir del 2026 solamente se rinde el parcial práctico del segundo cuatrimestre. A continuación se detallan los temas y su manera de encararlos a la hora de la resolución.

**Autores:**
* Mateo Lopez
* Agustin Carrasco
* Lautaro Sandoval

---

## Pasos para resolver

1. **Diagrama de Casos de Uso:** Se realiza el diagrama con todos los casos de uso del escenario planteado.
2. **Descripción de Caso de Uso:** Se hace la descripción textual del caso de uso indicado (ej: CU1).
3. **Realización del Caso de Uso:** Se compone de los siguientes elementos para dicho CU:
   1. **Diagrama de Clases:** Únicamente de las clases involucradas en el caso de uso analizado.
   2. **Tarjetas CRC:** De las clases utilizadas en el diagrama de clases del caso de uso.
   3. **Diagrama de Secuencia:** Del flujo temporal de ese caso de uso.

---

## Diagrama de Casos de Uso

Es un mapa visual que delimita los límites del sistema, quién interactúa con él (Actores) y qué funcionalidades de valor ofrece (Casos de Uso), sin entrar en el orden cronológico de las acciones.

### Elementos Clave

* **Actores:**
  * Son los que inician el caso de uso (actores del lado izquierdo).
  * Representan un rol externo al sistema, jamás un nombre propio (ej: *Cajero*, *Cliente*, *Administrador*; no *Juan Pérez*).
  * Pueden ser sistemas externos que envían o reciben datos (ej: *AFIP*).
  * Los actores del lado derecho suelen ser externos al sistema y actúan como soporte o receptores de información sin iniciar el caso de uso.
* **Casos de Uso (Óvalos):**
  * Nomenclatura: `[Verbo en infinitivo] + [Sustantivo]` (ej: *Registrar Pedido*).
* **Asociación (Línea sólida simple):**
  * Conecta al actor con el óvalo del caso de uso en el que participa.
  * **No lleva flechas.**
* **Límite del Sistema (System Boundary):**
  * Un rectángulo grande con el nombre del sistema arriba.
  * **Regla de oro:** Los casos de uso van adentro; los actores van siempre afuera.
  * *Nota de cátedra:* Preguntar en clase/parcial si es obligatorio graficarlo, aunque conceptualmente es correcto incluirlo.

### Relaciones en Casos de Uso

| Relación | Cuándo se usa | Sentido de la flecha |
| :--- | :--- | :--- |
| `<<include>>` | Parte obligatoria o común que se extrae para reutilizar. El CU base **no puede completarse** sin este. | Línea punteada que apunta hacia el caso de uso incluido (`CU Base -> CU Incluido`). |
| `<<extend>>` | Comportamiento opcional o condicional que ocurre bajo cierta regla/evento. | Línea punteada que apunta hacia el caso de uso base (`CU Extensión -> CU Base`). |
| **Generalización (Herencia)** | Un actor o caso de uso hereda el comportamiento de otro más general (ej: *Usuario Registrado* hereda de *Usuario*). | Línea sólida con flecha triangular hueca apuntando hacia el padre. |

### Errores Típicos a Evitar

* **Casos de uso "huérfanos":** Dejar un óvalo sin conectar a ningún actor.
* **Conectar actores entre sí con líneas simples:** Entre actores sólo puede haber relación de generalización/herencia (flecha triangular hueca).
* **Descomposición funcional:** Crear casos de uso para pasos individuales dentro de un proceso más grande (ej: crear *Validar Contraseña* o *Buscar Producto* como casos de uso aislados, en lugar de integrarlos dentro del flujo de *Iniciar Sesión* o *Registrar Venta*).
* **Invertir las flechas de include y extend:**
  * El base necesita/incluye al otro: la flecha apunta al CU incluido.
  * La extensión agrega algo a la base: la flecha apunta a la base.

---

## Descripción de Caso de Uso

En la descripción textual (o plantilla de caso de uso) se explica paso a paso la interacción entre el actor y el sistema.

* **Actor:** El rol que inicia la acción (ej: *Cajero*).
* **Precondición:** Qué debe ser verdad en el sistema antes de empezar (ej: *El usuario debe estar autenticado en el sistema*).
  * **Regla:** Lo que se especifica en la precondición **no se vuelve a indicar en el camino**.
  * *Ejemplo:* Si la precondición es *"Alumno autenticado"*, no se debe poner en el paso 1 *"El alumno ingresa usuario y contraseña"* ni en el paso 2 *"El sistema valida usuario y contraseña"*, ya que eso ocurrió previamente.
* **Postcondición:** Estado en el que queda el sistema tras completar el objetivo (ej: *La venta queda registrada y el stock actualizado*).
  * **OJO:** Precondición y Postcondición siempre se refieren al **sistema**, nunca a estados del usuario.
* **Camino Estándar (Flujo Principal):**
  * Los pasos alternan entre acción del actor y respuesta del sistema (uno y uno: Actor, Sistema, Actor, Sistema...).
  * Usa lenguaje generalista: *"ingresar datos"*, *"validar"*, *"solicitar confirmación"*.
  * **Prohibido incluir detalles técnicos o de interfaz gráfica:** No escribir *"el usuario presiona el botón X"* o *"se ejecuta un INSERT en la tabla Y"*. Usar en su lugar: *"el actor confirma la operación"* o *"el sistema registra la venta"*.
* **Caminos Alternativos:**
  * Desvíos del camino feliz donde algo falla o se toma otra opción válida (ej: datos incorrectos, falta de stock, cancelación).
  * Todo camino alternativo debe indicar explícitamente:
    * Si vuelve al flujo estándar (ej: *Retoma en el paso 3*).
    * Si termina el caso de uso sin éxito (ej: *Fin del caso de uso*).
  * **Numeración:** Si el desvío ocurre en el paso 4:
    * `4.a No hay evaluaciones pendientes`
      1. El sistema informa que no encuentra evaluaciones pendientes para el alumno.
      2. Finalizar caso de uso / ir a paso X.

### Errores Comunes en la Descripción
* **Olvidar al Actor:** Todo caso de uso debe estar iniciado directa o indirectamente por un actor.
* **Mezclar diseño técnico con requerimientos:** Hablar de pantallas, botones, tecnologías o bases de datos en la descripción funcional.

---

## Realización del Caso de Uso

Se trabaja exclusivamente sobre el caso de uso seleccionado para su análisis y resolución.

### 1. Diagrama de Clases

Representa únicamente las clases necesarias para ejecutar ese flujo específico:

* **Clases involucradas:**
  * Clase de interfaz (`UI`).
  * Clase controladora (`CTRL`).
  * Clase del sistema (`Sistema`).
  * Clases de dominio necesarias (con nombre, atributos necesarios y métodos).
* **Multiplicidad:** Indica cuántos objetos participan en la relación (ej: `1`, `*`, `0..1`).

#### Tipos de Relaciones en Clases

| Relación | Símbolo UML | Cuándo usarla |
| :--- | :--- | :--- |
| **Herencia (Generalización)** | Línea sólida con triángulo hueco hacia el padre | Relación *"es un"* (ej: *Docente es una Persona*). Hereda atributos y métodos. |
| **Composición** | Línea sólida con rombo relleno en el "todo" | El ciclo de vida de las partes depende fuertemente del todo. |
| **Agregación** | Línea sólida con rombo hueco en el contenedor | Contenedor global o asociación débil entre clases. |
| **Asociación simple** | Línea sólida | Relación estructural estándar entre dos entidades independientes. |
| **Dependencia** | Línea punteada con flecha | Cuando un elemento depende de otro para operar (ej: una clase usa a otra como parámetro en un método). |
| **Realización** | Línea punteada con triángulo hueco | Cuando una clase implementa una interfaz. |

#### Visibilidad

| Nivel | Símbolo | Definición |
| :--- | :---: | :--- |
| **Public** | `+` | Cualquier clasificador externo con visibilidad hacia la clase puede utilizar la característica. |
| **Protected** | `#` | Cualquier descendiente del clasificador (subclase) puede utilizar la característica. |
| **Private** | `-` | Sólo la propia clase puede utilizar la característica. |
| **Package** | `~` | Sólo los clasificadores declarados en el mismo paquete pueden utilizar la característica. |

*(Nota: Alcance de instancia/estático y clases DTO no se evalúan en la práctica).*

---

### Responsabilidades por Tipo de Clase

#### Clases UI y Controlador
> **Importante:** Las clases `UI` y `Controlador` **no llevan atributos**.

* **UI (Interfaz de Usuario):** Responsable de la interacción con el usuario, sin involucrarse en reglas de negocio.
  * Métodos típicos: Recibir datos del actor, mostrar información, mostrar confirmaciones o errores, solicitar datos o selecciones.
* **CTRL (Controlador):** Coordina la ejecución del caso de uso. Organiza la UI, el Sistema y las clases de dominio.
  * Métodos típicos: Iniciar una acción del CU, recibir acciones de la UI, coordinar el flujo de acciones, enviar mensajes al Sistema o clases de dominio, recibir resultados y continuar el flujo.
* **Sistema:** Representa la clase con visión global de la aplicación. Conoce las colecciones principales de objetos.
  * Métodos típicos: Mantiene/conoce las colecciones globales, busca objetos dentro de esos conjuntos, devuelve listas de objetos, realiza altas de nuevos objetos, ejecuta cálculos globales.
* **Dominio:** Representan entidades del negocio (*Cliente*, *Producto*, *Materia*).
  * **Sí contienen atributos y métodos.**
  * Realizan operaciones y validaciones sobre su propia información.

#### Guía Rápida para Asignar Métodos
1. ¿Es una interacción con el actor? -> **UI**
2. ¿Es coordinación del flujo del CU? -> **CTRL**
3. ¿Necesita buscar en un conjunto global, dar de alta o calcular a nivel general? -> **SISTEMA**
4. ¿Es información o comportamiento propio de un objeto concreto? -> **Clase de Dominio**
5. Si un mensaje llega a una clase en el diagrama de secuencia -> **Debe figurar como método en esa clase en el diagrama de clases.**

---

### 2. Tarjetas CRC (Clase - Responsabilidad - Colaboración)

Definen el comportamiento y las interacciones de cada clase del caso de uso.

#### Estructura de la Tarjeta

##### Anverso (Frente)
* **Clase:** Nombre único de la clase.
* **Propósito:** Descripción breve y clara del rol que cumple dentro del dominio del problema.

```text
+-----------------------------------------------------------+
| Clase: [Nombre de la Clase]                               |
| Propósito: [Breve descripción de su rol en el sistema]    |
+-----------------------------------------------------------+
```

##### Reverso (Dorso)
* **Responsabilidades:** Qué tareas sabe realizar y qué información conoce (métodos clave y datos que administra).
* **Colaboración:** Otras clases con las que interactúa para cumplir esas responsabilidades.

```text
+-----------------------------+-----------------------------+
| Responsabilidades:          | Colaboración:               |
+-----------------------------+-----------------------------+
| - [Tarea que realiza]       | - [Clase colaboradora]      |
| - [Información que conoce]  |                             |
+-----------------------------+-----------------------------+
```

#### Consejos para Tarjetas CRC
* **Coherencia absoluta:** Cada clase que aparezca en el Diagrama de Clases debe tener obligatoriamente su tarjeta CRC.
* **Simplicidad:** Si el reverso acumula demasiadas responsabilidades, evalúa si la clase viola el Principio de Responsabilidad Única (SRP) y debe dividirse.
* **Foco en el CU:** Listar solo las responsabilidades que la clase ejecuta en el caso de uso analizado.

---

### 3. Diagrama de Secuencia

* Modela el intercambio de mensajes entre objetos ordenado estrictamente en una **secuencia temporal** (eje vertical descendente).
* Representa al sistema en tiempo de ejecución.
* Muestra cómo colaboran las instancias (`UI`, `CTRL`, `Sistema`, objetos de dominio) mediante la invocación de métodos para concretar el flujo del caso de uso.