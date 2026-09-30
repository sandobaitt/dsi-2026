# Descripción de Caso de Uso: Reasignar Tickets por Ausencia de Desarrollador

* **Actor Primario:** Líder Técnico
* **Actor Secundario:** Trello
* **Precondiciones:**
  * El Líder Técnico se encuentra autenticado en el sistema.
  * Existe al menos un desarrollador del equipo con tickets activos asignados.
* **Poscondiciones:**
  * Los tickets seleccionados quedan reasignados al nuevo desarrollador indicado.
  * Queda registrado el responsable y la fecha de cada reasignación realizada.

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Sistema** | El sistema muestra las opciones disponibles al Líder Técnico. |
| **2** | **Actor** | El Líder Técnico selecciona la opción "Reasignar tickets por ausencia". |
| **3** | **Sistema** | El sistema muestra la lista de desarrolladores del equipo. |
| **4** | **Actor** | El Líder Técnico selecciona el desarrollador que se ausenta. |
| **5** | **Sistema** | El sistema busca los tickets activos (abiertos o en progreso) asignados a ese desarrollador. |
| **6** | **Sistema** | El sistema consulta a Trello por los estados de tarjeta de los tickets encontrados y devuelve solo los tickets candidatos a reasignación. |
| **7** | **Actor** | El Líder Técnico selecciona un ticket de la lista. |
| **8** | **Sistema** | El sistema muestra la lista de desarrolladores disponibles para asignar. |
| **9** | **Actor** | El Líder Técnico selecciona el nuevo desarrollador para ese ticket. |
| **10** | **Sistema** | El sistema actualiza el ticket con el nuevo desarrollador asignado. |
| **11** | **Sistema** | El sistema registra el cambio realizado, indicando responsable y fecha. |
| **12** | **Sistema** | El sistema repite los pasos 7 a 11 hasta que el Líder Técnico decide finalizar la reasignación (*). |
| **13** | **Sistema** | El sistema muestra un resumen de los tickets reasignados. |

> **(\*) Nota de especificación:** En el texto oficial de la cátedra se referencia el ciclo como *"repite los pasos 9 a 13 hasta que el Líder Técnico decide finalizar"*. Desde el punto de vista procedimental y de interfaz, el bucle de reasignación individual de tareas comprende la selección de un nuevo ticket (paso 7) hasta el registro de la asignación (paso 11), permitiendo reasignar múltiples tickets antes de pasar a la visualización del resumen consolidado (paso 13).

---

### Caminos Alternativos

#### 5.a El desarrollador seleccionado no posee tickets activos asignados
1. El sistema informa que el desarrollador seleccionado no tiene tickets en estado abierto o en progreso.
2. Retoma en el paso 3.

#### 6.a Los tickets activos en DevTrack figuran como "Hecho" en Trello
1. El sistema constata que todas las tarjetas asociadas a los tickets del desarrollador ausente se encuentran en la columna "Hecho" de Trello, no requiriendo reasignación.
2. El sistema informa la situación y retoma en el paso 3.

#### 6.b Error de comunicación con Trello
1. El sistema informa que el servicio externo de Trello no responde o ha retornado un error de conexión.
2. El sistema permite reintentar la sincronización o continuar utilizando el estado local de DevTrack.
3. Retoma en el paso 6 o Fin del caso de uso.

#### 8.a No existen desarrolladores disponibles en el equipo
1. El sistema advierte que el resto de los integrantes del equipo presentan una carga de trabajo completa o sobrecarga para el sprint vigente.
2. El Líder Técnico puede forzar la asignación asumiendo la sobrecarga o desestimar la reasignación para dicho ticket.
3. Retoma en el paso 7 o Fin del caso de uso.
