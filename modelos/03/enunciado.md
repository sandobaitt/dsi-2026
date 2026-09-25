# Modelo 03: Fábrica de Muebles Metálicos (Armar Hoja de Ruta)

## Consigna (Ejercicio 3 / Ejercicio 6 de la guía)

Considere el ejercicio 6 de la guía "Modelado de CU", haga la realización del caso de uso "Armar hoja de ruta", realice sólo el camino estándar.

### Enunciado del Ejercicio 6

El dueño de una fábrica de muebles metálicos quiere organizar la forma de trabajo introduciendo entre otras mejoras a su sistema, la generación de hojas de ruta para las operaciones diarias.

La mecánica que tienen en mente consiste en que el jefe de taller diariamente visualice los pedidos para poder satisfacerlos, estos se componen de diferentes tipos de muebles (armarios, mesas, sillas, etc), cada uno de estos cuenta con un conjunto de tareas estándar (plantilla de tareas) que son cargadas a la hoja de ruta para evitar su relleno desde cero una y otra vez. De acuerdo con las especificaciones del pedido se agregan, modifican o borran tareas indicándose además su duración, finalmente se asignan uno o más operarios disponibles para que efectúen el trabajo requerido, automáticamente se calcula el tiempo total estimado, y se imprimen las copias necesarias. Cabe aclarar que las tareas están relacionadas con diferentes secciones del taller y cada operario pertenece a una de estas secciones.

El jefe de taller mencionó que sería muy útil realizar un control posterior a la jornada, donde cada operario busca su hoja de ruta y marca las tareas que fueron completadas.

El dueño también requiere la implantación de la carga de pedidos entonces pueden ser controlados de manera más eficiente, ya que cada pedido puede contar con más de un tipo de mueble, y llevar varios días para completarlo.

---

## Actividades Solicitadas

1. **Descripción de Caso de Uso:** "Armar Hoja de Ruta" (camino estándar).
2. **Diagrama de Clases:** Realización del CU "Armar Hoja de Ruta" con atributos y métodos (basado en el diseño estructural de la cátedra: `UISolHR`, `CTRLSolHR`, `Sistema`, `Pedido`, `Linea_pedido`, `Mueble`, `Plantilla_tarea`, `Tarea`, `Seccion`, `Operario`, `Hoja de Ruta`).
3. **Diagrama de Secuencia:** Realización del camino estándar del caso de uso.
