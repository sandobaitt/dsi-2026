# Plantilla: Descripción de Caso de Uso

> **Pauta de Cátedra (DSI 2026):**
> - **Precondición y Postcondición:** Se refieren siempre al **sistema**, nunca a estados del usuario.
> - **Regla de oro:** Lo especificado en la precondición **no se vuelve a indicar en los pasos** (ej: si la precondición es "Usuario autenticado", no agregar pasos de login ni validación de contraseña).
> - **Lenguaje generalista:** Prohibido incluir detalles técnicos o de interfaz gráfica (no escribir *"el usuario presiona el botón X"*, *"pantalla"*, ni *"INSERT"*). Usar *"el actor solicita..."*, *"el actor confirma..."*, *"el sistema registra..."*.
> - **Alternancia estricta:** Los pasos del camino estándar alternan uno a uno (Actor, Sistema, Actor, Sistema...).
> - **Caminos alternativos:** Numerados como `[Paso].[letra]` (ej. `4.a`). Deben indicar explícitamente si finalizan el caso de uso (*Fin del caso de uso*) o en qué paso retoman (*Retoma en el paso X*).

---

## Caso de Uso: [CU-XX] [Verbo en infinitivo + Sustantivo]

* **Actor:** [Rol que inicia el caso de uso, ej. Cajero, Cliente]
* **Precondición:** [Estado que debe cumplir el sistema antes de iniciar, ej. El usuario se encuentra autenticado en el sistema]
* **Postcondición:** [Estado en el que queda el sistema tras completar el objetivo, ej. La operación queda registrada y el saldo actualizado]

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | Solicita iniciar la operación... |
| **2** | **Sistema** | Solicita los datos requeridos para la operación. |
| **3** | **Actor** | Ingresa los datos solicitados y confirma. |
| **4** | **Sistema** | Valida los datos ingresados. |
| **5** | **Sistema** | Registra la información y actualiza el estado correspondiente. |
| **6** | **Sistema** | Informa la confirmación de la operación exitosa. |

*(Nota: Los pasos alternan entre acción del actor y respuesta del sistema).*

---

### Caminos Alternativos

#### 4.a [Condición de fallo o desvío, ej. Datos inválidos o faltantes]
1. El sistema informa el motivo del error.
2. Retoma en el paso 3 / Fin del caso de uso.

#### 4.b [Condición alternativa, ej. Fondos o existencias insuficientes]
1. El sistema notifica la imposibilidad de completar la operación.
2. Fin del caso de uso.
