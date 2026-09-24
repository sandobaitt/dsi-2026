# Modelo de Examen 02: DevTrack (NubeSoft)

## Enunciado

NubeSoft es una empresa de desarrollo de software organizada en equipos pequeños que trabajan en distintos proyectos para varios clientes. Actualmente el seguimiento del trabajo se hace con Trello (cada equipo mueve tarjetas entre "Por hacer", "En progreso" y "Hecho") y, por separado, con un sistema de integración continua que ejecuta los pipelines de build y deploy. Esta información está repartida y no hay un lugar único que la centralice.

La empresa decide desarrollar **DevTrack**, un sistema interno de gestión de tickets que no reemplaza estas herramientas, sino que se apoya en ellas.

Cada desarrollador carga un ticket cuando empieza a trabajar en una tarea, con descripción, proyecto, prioridad y, opcionalmente, el enlace a la tarjeta de Trello asociada. A medida que avanza, actualiza el estado del ticket (abierto → en progreso → resuelto) y puede modificar su descripción o prioridad. Un ticket que deja de tener sentido puede cancelarse sin eliminarlo, para conservar el registro.

Los líderes técnicos necesitan ver todos los tickets de su equipo y poder filtrarlos por estado, prioridad o asignado, para detectar sobrecarga o tareas urgentes sin tomar. También necesitan una vista temporal: por ejemplo, los tickets del sprint actual, o aquellos que llevan abiertos más tiempo del esperado. Son ellos quienes asignan tickets sin dueño a un desarrollador y quienes cierran definitivamente un ticket una vez validado el trabajo.

El sistema también debe consultar información externa en tiempo real: para un ticket con tarjeta de Trello asociada, debe poder traer en qué columna está esa tarjeta ahora mismo; para un ticket cuyo cambio de código ya fue desplegado, debe poder consultar el resultado del último pipeline asociado.

Actualmente el equipo de NubeSoft está desarrollando una funcionalidad adicional pensada para situaciones de ausencia: cuando un desarrollador se toma vacaciones o licencia, el líder técnico necesita reasignar rápidamente todos sus tickets activos.

Para eso, el sistema deberá mostrarle al líder los tickets abiertos del desarrollador que se ausenta —verificando antes, contra Trello, que esos tickets sigan realmente activos y no hayan quedado desactualizados—, junto con la carga de trabajo actual del resto del equipo dentro del sprint vigente, para que el líder pueda decidir con criterio a quién reasignar cada ticket.

Un rol administrativo se encarga del alta de usuarios y la configuración general del sistema.

---

## Caso de Uso Analizado

**CU:** Reasignar Tickets por Ausencia de Desarrollador  
**Actor primario:** Líder Técnico  
**Actores secundarios:** Trello  
**Precondiciones:**
* El Líder Técnico se encuentra autenticado en el sistema.
* Existe al menos un desarrollador del equipo con tickets activos asignados.

**Poscondiciones:**
* Los tickets seleccionados quedan reasignados al nuevo desarrollador indicado.
* Queda registrado el responsable y la fecha de cada reasignación realizada.

### Camino Estándar
1. El sistema muestra las opciones disponibles al Líder Técnico.
2. El Líder Técnico selecciona la opción "Reasignar tickets por ausencia".
3. El sistema muestra la lista de desarrolladores del equipo.
4. El Líder Técnico selecciona el desarrollador que se ausenta.
5. El sistema busca los tickets activos (abiertos o en progreso) asignados a ese desarrollador.
6. El sistema consulta a trello por los estados de tarjeta de los tickets encontrados y devuelve solo los tickets candidatos a reasignación.
7. El Líder Técnico selecciona un ticket de la lista.
8. El sistema muestra la lista de desarrolladores disponibles para asignar.
9. El Líder Técnico selecciona el nuevo desarrollador para ese ticket.
10. El sistema actualiza el ticket con el nuevo desarrollador asignado.
11. El sistema registra el cambio realizado, indicando responsable y fecha.
12. El sistema repite los pasos 7 a 11 hasta que el Líder Técnico decide finalizar la reasignación.
13. El sistema muestra un resumen de los tickets reasignados.

---

## Actividades Desarrolladas

1. **Descripción de Caso de Uso:** Especificación formal con camino estándar y alternativos según pautas de cátedra DSI.
2. **Tarjetas CRC:** Elaboración de las tarjetas correspondientes a las entidades centrales del caso de uso.
3. **Diagrama de Clases:** Modelado estructural con paquetes, visibilidad, métodos y reglas de cátedra (UI y CTRL sin atributos).
4. **Diagrama de Secuencia:** Diagrama temporal con integración de `CTRLSesion`, fachada `Sistema`, actor externo `Trello`, listas temporales y creación de registros de reasignación.
