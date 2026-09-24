# Descripción de Caso de Uso: Reasignar Tickets por Ausencia de Desarrollador

* **Actor Primario:** Líder Técnico
* **Actor Secundario:** Trello
* **Precondición:** El Líder Técnico se encuentra autenticado en el sistema y existe al menos un desarrollador en su equipo con tickets activos asignados.
* **Postcondición:** Los tickets seleccionados quedan reasignados al nuevo desarrollador indicado y queda registrada la reasignación con responsable, fecha y trazabilidad de cambios.

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | Solicita la opción de reasignar tickets por ausencia de desarrollador. |
| **2** | **Sistema** | Recupera los desarrolladores pertenecientes al equipo a cargo del Líder Técnico y solicita seleccionar al desarrollador que se ausenta. |
| **3** | **Actor** | Selecciona el desarrollador que se ausenta. |
| **4** | **Sistema** | Busca los tickets activos (abiertos o en progreso) asignados a dicho desarrollador, consulta a Trello el estado en tiempo real de las tarjetas asociadas y muestra la lista de tickets candidatos a reasignación. |
| **5** | **Actor** | Selecciona un ticket a reasignar de la lista. |
| **6** | **Sistema** | Muestra la lista de desarrolladores del equipo disponibles para asignación junto con su carga de trabajo actual dentro del sprint vigente. |
| **7** | **Actor** | Selecciona el nuevo desarrollador para el ticket seleccionado. |
| **8** | **Sistema** | Actualiza la asignación del ticket y registra la reasignación indicando responsable (Líder Técnico), desarrollador anterior, nuevo desarrollador y fecha/hora. |
| **9** | **Actor** | Repite los pasos 5 a 8 para reasignar otros tickets y confirma la finalización del proceso. |
| **10** | **Sistema** | Muestra el resumen consolidado de los tickets reasignados con sus nuevos responsables. |

---

### Caminos Alternativos

#### 4.a El desarrollador no posee tickets candidatos activos
1. El sistema informa que el desarrollador seleccionado no tiene tickets en estado abierto/en progreso, o que todas sus tarjetas asociadas en Trello figuran en estado "Hecho".
2. Retoma en el paso 3.

#### 4.b Error de comunicación con Trello
1. El sistema informa que el servicio externo de Trello no responde o no se encuentra disponible momentáneamente.
2. El sistema permite reintentar la conexión o utilizar el estado local registrado en DevTrack.
3. Retoma en el paso 4 o Fin del caso de uso.

#### 6.a No existen desarrolladores disponibles en el equipo
1. El sistema advierte que todos los desarrolladores del equipo presentan sobrecarga de tareas para el sprint vigente.
2. El Líder Técnico puede forzar la asignación asumiendo la sobrecarga o cancelar la operación para dicho ticket.
3. Retoma en el paso 5.
