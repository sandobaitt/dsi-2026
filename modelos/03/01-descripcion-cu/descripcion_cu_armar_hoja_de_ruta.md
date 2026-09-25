# Descripción de Caso de Uso: Armar Hoja de Ruta

* **Actor:** Jefe de Taller
* **Precondición:** El usuario se encuentra autenticado en el sistema y existen pedidos pendientes de producción con sus respectivos muebles asociados.
* **Postcondición:** La Hoja de Ruta queda registrada en el sistema y se emiten las copias impresas para las operaciones diarias del taller.

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | El Jefe de Taller solicita armar la hoja de ruta diaria. |
| **2** | **Sistema** | El sistema busca y muestra los diferentes muebles correspondientes a los pedidos pendientes de fabricación. |
| **3** | **Actor** | El Jefe de Taller selecciona el mueble requerido para la orden de trabajo. |
| **4** | **Sistema** | El sistema carga las tareas estándar asociadas a la plantilla del mueble seleccionado, calcula el tiempo total estimado y presenta la vista previa de la hoja de ruta. |
| **5** | **Actor** | El Jefe de Taller confirma la hoja de ruta. |
| **6** | **Sistema** | El sistema registra la nueva hoja de ruta y emite la impresión de las copias correspondientes para los operarios de las secciones. |

*(Fin del caso de uso)*
