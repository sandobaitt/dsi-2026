---
name: dsi-descripcion-cu
description: >-
  Guía y pautas obligatorias de cátedra para la Descripción y Especificación Textual de Casos de Uso
  en Diseño de Sistemas (DSI 2026). Aplica reglas de pre/postcondición referidas exclusivamente al sistema,
  prohibición de repetir la precondición en los pasos, alternancia estricta 1 a 1 Actor/Sistema,
  prohibición de detalles técnicos/botones/UI, ingreso múltiple iterativo (el actor no ingresa listas), postcondición solo si hay
  cambio de estado y numeración formal de caminos alternativos (ej: 4.a).
---

# Skill: Descripción de Caso de Uso - Pautas de Cátedra (DSI 2026)

Esta skill establece las normas estrictas de redacción y corrección de la cátedra de **Diseño de Sistemas (DSI 2026)** para la elaboración de la **Descripción Textual de Casos de Uso**.

---

## 📌 Reglas de Oro de la Cátedra (Checklist de Parcial)

### 1. Precondiciones y Postcondiciones
- **Referidas siempre al SISTEMA:** Precondición y postcondición expresan estados del sistema, **nunca** sensaciones o estados del usuario (ej. *"El usuario está contento"* es incorrecto; *"La orden queda registrada"* es correcto).
- **Prohibido repetir la precondición en el camino:**
  - Si la precondición es *"El usuario se encuentra autenticado en el sistema"*, **está terminantemente prohibido** poner en el paso 1 *"El usuario ingresa login y contraseña"* ni en el paso 2 *"El sistema valida credenciales"*. Eso ya ocurrió previamente.
  - **La precondición es independiente del camino:** si algo ya está garantizado por la precondición, tampoco se repite en los caminos alternativos.
- **La postcondición solo se escribe si la ejecución produce un cambio de estado interno** en el sistema (se registra una venta, se actualiza el stock). Si el CU solo consulta o muestra información y no modifica nada, **no lleva postcondición**.

### 2. Lenguaje Generalista (Prohibición de Términos de UI y Técnicos)
- **Prohibido incluir botones, pantallas o tecnologías:**
  - ❌ *Incorrecto:* *"El usuario presiona el botón Guardar"*, *"Se muestra la pantalla X"*, *"Se ejecuta un INSERT en la tabla Y"*.
  - ✔️ *Correcto:* *"El actor confirma la operación"*, *"El sistema solicita los datos..."*, *"El sistema registra la venta"*.
- Usar verbos de intención: *"ingresar datos"*, *"validar"*, *"solicitar confirmación"*, *"informar resultado"*.

### 3. Alternancia Estricta 1 a 1 (Actor / Sistema)
- El flujo estándar debe alternar rigurosamente: **Paso 1 (Actor) $\rightarrow$ Paso 2 (Sistema) $\rightarrow$ Paso 3 (Actor) $\rightarrow$ Paso 4 (Sistema)...**
- **Ingreso múltiple:** el actor **no puede ingresar listas o estructuras de datos al sistema de una sola vez**. La selección múltiple se modela de forma iterativa: el sistema muestra las opciones, el actor selecciona y envía **un** elemento, el sistema responde, y el ciclo se repite hasta que el actor indica que terminó.
  - ❌ *"El actor ingresa los ejemplares que desea llevar."*
  - ✔️ *"3. El sistema solicita un ejemplar. 4. El actor ingresa el código de un ejemplar. 5. El sistema registra el ejemplar y vuelve a solicitar otro (se repiten los pasos 4 y 5 hasta que el actor indica que terminó)."*

### 4. Caminos Alternativos (Formato y Numeración de Cátedra)
- Todo desvío o error debe estar identificado con el número del paso donde ocurre más una letra minúscula (ej. `4.a`).
- **Obligatoriedad de cierre:** Todo camino alternativo debe indicar explícitamente una de dos opciones:
  - `Retoma en el paso X.`
  - `Fin del caso de uso.`

---

## 📝 Estructura Obligatoria de Entrega

```markdown
# Descripción de Caso de Uso: [CU-XX] [Verbo + Sustantivo]

* **Actor:** [Rol]
* **Precondición:** [Estado que cumple el sistema antes de empezar]
* **Postcondición:** [Estado en el que queda el sistema tras completar el objetivo. Se omite si el CU solo consulta información]

---

### Camino Estándar

| Paso | Responsable | Acción / Proceso |
| :---: | :---: | :--- |
| **1** | **Actor** | Solicita... |
| **2** | **Sistema** | Solicita los datos... |
| **3** | **Actor** | Ingresa los datos y confirma... |
| **4** | **Sistema** | Valida los datos... |
| **5** | **Sistema** | Registra los cambios... |
| **6** | **Sistema** | Informa el resultado exitoso... |

---

### Caminos Alternativos

#### 4.a [Condición de desvío o error]
1. El sistema informa el motivo del error.
2. Retoma en el paso 3 / Fin del caso de uso.
```
