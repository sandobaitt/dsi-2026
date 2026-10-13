# Descripción de Caso de Uso: Registrar Préstamo

* **Actor Primario:** Bibliotecario
* **Actor Secundario:** Sistema de Alumnado
* **Precondición:** El bibliotecario se encuentra autenticado en el sistema.
* **Postcondición:** El préstamo queda registrado a nombre del estudiante, con la fecha límite de devolución de cada ejemplar según el tipo de préstamo, y los ejemplares prestados quedan en estado prestado.

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | Solicita registrar un préstamo. |
| **2** | **Sistema** | Solicita el legajo del estudiante. |
| **3** | **Actor** | Ingresa el legajo del estudiante. |
| **4** | **Sistema** | Verifica con el sistema de Alumnado que el estudiante sea regular, y que no tenga libros vencidos sin devolver ni una sanción vigente. Solicita el tipo de préstamo (a domicilio o en sala) y los ejemplares que desea llevar. |
| **5** | **Actor** | Selecciona el tipo de préstamo e ingresa los ejemplares que desea llevar. |
| **6** | **Sistema** | Verifica que los ejemplares estén disponibles y que, sumados a los que el estudiante ya tiene prestados, no superen los tres ejemplares. Presenta el resumen del préstamo y solicita confirmación. |
| **7** | **Actor** | Confirma el préstamo. |
| **8** | **Sistema** | Registra el préstamo con la fecha límite de devolución de cada ejemplar según el tipo de préstamo, cambia el estado de los ejemplares a prestado e informa el resultado. |

*(Fin del caso de uso)*

---

### Caminos Alternativos

#### 4.a El legajo no corresponde a un estudiante
1. El sistema informa que el legajo ingresado no corresponde a ningún estudiante.
2. Retoma en el paso 2.

#### 4.b El estudiante no es regular
1. El sistema informa que el estudiante no tiene la condición de regular y que no puede registrarse el préstamo.
2. Fin del caso de uso.

#### 4.c El estudiante tiene libros vencidos sin devolver
1. El sistema informa que el estudiante tiene libros vencidos sin devolver y que no puede registrarse el préstamo.
2. Fin del caso de uso.

#### 4.d El estudiante tiene una sanción vigente
1. El sistema informa que el estudiante se encuentra sancionado y la fecha en que finaliza la sanción.
2. Fin del caso de uso.

#### 4.e El sistema de Alumnado no responde
1. El sistema informa que no pudo obtener la situación académica del estudiante y que no puede registrarse el préstamo.
2. Fin del caso de uso.

#### 6.a Algún ejemplar no está disponible
1. El sistema informa cuáles de los ejemplares ingresados no están disponibles (prestados, en reparación o dados de baja).
2. Retoma en el paso 5.

#### 6.b El préstamo supera el máximo de tres ejemplares
1. El sistema informa que, con los ejemplares ingresados, el estudiante superaría el máximo de tres ejemplares prestados al mismo tiempo, e indica cuántos puede llevar aún.
2. Retoma en el paso 5.

#### 7.a El bibliotecario cancela el préstamo
1. El sistema descarta el préstamo sin modificar el estado de los ejemplares.
2. Fin del caso de uso.
