# Ejercicio Extra 2026: BiblioUTN

> UTN - FRRe · Diseño de Sistemas · Ejercicio extra de práctica para el parcial.
>
> **Alcance resuelto en esta carpeta:** Ejercicio 1 y Ejercicio 2. Los ejercicios 3 a 6 están enunciados abajo, pero todavía no se resolvieron.

## Enunciado

Cada cuatrimestre, la Biblioteca de la Facultad Regional Resistencia cierra el año con un faltante de ejemplares que nadie sabe explicar. El último inventario detectó más de cuarenta libros sin devolver, varios de ellos prestados a personas que se habían recibido o dejado la carrera hacía meses. El problema es que el préstamo se registra en fichas de papel y la condición de estudiante regular se da por supuesta: el bibliotecario no tiene forma rápida de consultarla.

Para resolverlo, la Secretaría Académica encarga **BiblioUTN**, una aplicación para el mostrador de la biblioteca que lleva el control del catálogo y de los préstamos, y que obtiene la situación académica de cada estudiante directamente del sistema de Alumnado, que sigue siendo el responsable de esa información.

La biblioteca tiene un catálogo de libros. De cada libro se registran título, autores, editorial y año, y puede haber varios ejemplares físicos, cada uno identificado por un código de inventario. Un ejemplar puede estar disponible, prestado, en reparación o dado de baja. Cada estudiante se identifica por su número de legajo. Un préstamo pertenece a un estudiante y puede incluir varios libros, cada uno con su fecha límite de devolución, que depende del tipo de préstamo: a domicilio (7 días) o en sala (mismo día).

El equipo de desarrollo trabaja en tres funcionalidades:

**Registrar préstamo.** Cuando un estudiante se presenta en el mostrador, el bibliotecario ingresa su legajo y los ejemplares que quiere llevar. El sistema verifica con el sistema de Alumnado que sea estudiante regular. Si no lo es, no se presta. Tampoco se presta a un estudiante que tenga libros vencidos sin devolver. Un estudiante no puede tener más de tres ejemplares prestados al mismo tiempo. Al confirmar el préstamo, los ejemplares pasan a estado prestado.

**Registrar devolución.** El estudiante puede devolver todos los ejemplares de un préstamo juntos o de a uno. Si devuelve fuera de término, queda sancionado sin poder pedir préstamos por tantos días como días de atraso tuvo. La biblioteca quiere conservar el historial de sanciones de cada estudiante: motivo, fecha de inicio y fecha de fin.

**Renovar préstamo.** El estudiante puede pedir extender la fecha de devolución de uno o más ejemplares, siempre que no estén vencidos y que siga siendo estudiante regular al momento de renovar. Cada ejemplar se puede renovar como máximo dos veces. Se debe registrar cada renovación: fecha anterior, fecha nueva y quién la realizó.

Un rol administrativo se encarga del alta de bibliotecarios, del catálogo de libros y del registro de ejemplares.

---

## Actividades Solicitadas

### Ejercicio 1
Realizar el Diagrama de CU completo.

### Ejercicio 2
* **a.** Realizar la descripción del CU "Registrar préstamo". Describir al menos 2 caminos alternativos.
* **b.** Realización del CU "Registrar préstamo":
  * **i.** Confeccionar las tarjetas CRC.
  * **ii.** Realizar el Diagrama de clases con atributos, métodos, asociaciones, cardinalidad y navegabilidad. Derivar el pseudocódigo a partir del diagrama de clases.
  * **iii.** Desarrollar el diagrama de secuencia UML correspondiente al camino estándar del CU.

### Ejercicio 3 *(pendiente)*
* **a.** Realizar la descripción del CU "Registrar devolución". Describir al menos 2 caminos alternativos.
* **b.** Realización del CU "Registrar devolución": tarjetas CRC, diagrama de clases (con atributos, métodos, asociaciones, cardinalidad y navegabilidad; derivar el pseudocódigo) y diagrama de secuencia del camino estándar.

### Ejercicio 4 *(pendiente)*
* **a.** Realizar la descripción del CU "Renovar Préstamo". Describir al menos 2 caminos alternativos.
* **b.** Realización del CU "Renovar Préstamo": tarjetas CRC, diagrama de clases (con atributos, métodos, asociaciones, cardinalidad y navegabilidad; derivar el pseudocódigo) y diagrama de secuencia del camino estándar.

### Ejercicio 5 *(pendiente)*
Los estudiantes se quejan de que los libros más pedidos nunca están disponibles. Por eso, la biblioteca incorpora una **lista de espera**: cuando un estudiante busca un libro y no hay ejemplares disponibles, puede anotarse para que le avisen cuando se libere uno. Cuando se registra la devolución de un ejemplar y este queda disponible, deben ocurrir varias cosas:

* Se avisa por correo electrónico a los estudiantes anotados en la lista de espera de ese libro.
* Se envía también una notificación a la app móvil de la facultad para quienes la tengan instalada.
* Se actualiza el panel de estadísticas de la biblioteca, que muestra en tiempo real la cantidad de ejemplares disponibles por libro.

La biblioteca ya anticipa que en el futuro querrá sumar otros avisos, por ejemplo, un mensaje por WhatsApp o un aviso al docente de la cátedra que recomienda el libro. Quiere poder agregarlos sin modificar la lógica de la devolución.

Se pide:
* **a)** Identificar qué patrón de diseño resuelve este problema y justificar su elección en relación con el requisito de agregar nuevos avisos sin modificar la devolución.
* **b)** Realizar el diagrama de clases de la solución.

### Ejercicio 6 *(pendiente)*
Modelar el diagrama de estado de la clase **EJEMPLAR**.
