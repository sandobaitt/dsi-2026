---
name: dsi-secuencia
description: >-
  Guía y pautas obligatorias de la cátedra para la resolución y diagramado de Diagramas de Secuencia
  y su coherencia con Diagramas de Clases en Diseño de Sistemas (DSI). Aplica reglas estrictas de
  CTRLSesion preexistente, patrón experto, creación de entidades/composiciones, listas temporales por Sistema,
  nomenclatura de mensajes, fragmentos de interacción y retornos implícitos para parciales.
---

# Skill: Diagramas de Secuencia - Pautas de Cátedra (DSI)

Esta skill define las reglas obligatorias, criterios de corrección y convenciones específicas de la cátedra de **Diseño de Sistemas (DSI)** para modelar **Diagramas de Secuencia** en PlantUML y asegurar su consistencia con el Diagrama de Clases.

---

## 📌 Reglas de Oro de la Cátedra (Checklist de Examen)

### 1. Manejo de Sesión y Credenciales (`CTRLSesion`)
- **Prohibido solicitar datos que el sistema ya conoce:** Nunca pedirle al actor que ingrese manualmente información de su propia sesión activa (ej: `legajo`, `idUsuario`, `rol`).
- **Instancia preexistente:** El objeto de sesión activa (`CTRLSesion`) debe modelarse como un objeto **preexistente / externo**. NUNCA usar `create` sobre la sesión.
- **Ubicación en el diagrama:** Se posiciona visualmente entre el Controlador del Caso de Uso (`CTRLCU`) y la Fachada del Sistema (`Sistema`):
  `Actor -> Pantalla (UI) -> CTRLCU -> CTRLSesion -> Sistema -> Entidades`
- **Comunicación:** El `CTRLCU` consulta a `CTRLSesion` para recuperar las credenciales (ej: `obtenerUsuarioLogueado()`).

### 2. Nomenclatura, Mensajes y Patrón Experto
- **De `CTRL` a `Sistema`:** Usar verbos de negocio en español (ej: `obtenerEmpleados()`, `registrarVenta()`). **Prohibido usar `get` a este nivel**.
- **De `Sistema` a `Entidad`:** Usar getters formales para consultar atributos (ej: `getNombre()`, `getCupo()`).
- **Paso de Parámetros:** Toda invocación de ida debe incluir sus parámetros en la firma (ej: `procesarPago(idMedio, datos)`).
- **Delegación y Patrón Experto:** **Prohibido** extraer datos con getters para hacer cálculos matemáticos en el `Sistema` o `CTRL` (ej: `getPrecio() * getCant()`). Los cálculos complejos y cambios de estado se delegan a la entidad experta mediante un mensaje de negocio (ej: `Entidad -> Entidad: calcularTotal()`).
- **Mensajes Reflexivos (Self-Message):** Utilizar invocaciones a la misma clase para cálculos o validaciones internas (`Sistema -> Sistema: validarEstructura()`).

### 3. Parámetros Obligatorios en los Métodos
- **Prohibido dejar paréntesis vacíos si la operación requiere datos:** Todo mensaje que envíe o necesite información para operar **debe incluir sus parámetros explícitamente** con nombres representativos.
  - ✔️ `add(mueble)`, `seleccionarMueble(idMueble)`, `obtenerPreVistaHR(idMueble)`, `calcularTiempoTotal(listaTareas)`, `decrementarCupo(cantidad)`.
  - ❌ `add()`, `seleccionarMueble()`, `obtenerPreVistaHR()`, `calcularTiempoTotal()`.
- **Coherencia con Diagrama de Clases:** Cada parámetro graficado en secuencia debe figurar con su tipo en la firma del método en la clase (`+ seleccionarMueble(idMueble: int): void`).

### 4. Ciclo de Vida de Objetos: Uso de `create()` y `destroy`

#### A. Cuándo y cómo usar `create()`
- **Cuándo se usa:** Siempre que una instancia **nace dinámicamente** durante el flujo (nuevas entidades permanentes, objetos de transacción, ítems de composición y listas temporales).
- **Sintaxis en PlantUML:**
  ```plantuml
  create nuevaInstancia
  Emisor -> nuevaInstancia: create(parametro1, parametro2)
  ```
  La directiva `create` hace que la línea de vida nazca visualmente en ese instante temporal y no desde la parte superior.
- **Parámetros en `create`:** Si el objeto requiere datos iniciales al construirse, deben especificarse: `create(mueble, tiempoTotal, listaTareas)`.
- **Objetos que NUNCA llevan `create`:** Instancias preexistentes como `CTRLSesion`, `Sistema`, `UI`, `CTRL` y colecciones ya existentes en memoria.

#### B. Cuándo y cómo usar `destroy`
- **Cuándo se usa:** Para marcar el **fin formal del ciclo de vida** de un objeto efímero (típicamente una `ListaTemporal`) cuando ya no es requerida por el sistema.
- **Sintaxis en PlantUML:**
  ```plantuml
  destroy listaTemporal
  ```
  PlantUML grafica automáticamente la cruz (`X`) de finalización al pie de la línea de vida.
- **Ubicación exacta:** Se coloca inmediatamente después de que el objeto que consumía la lista (ej. `Sistema` o `CTRL`) finaliza su uso o transmite los datos.
- **Pauta de parcial:** En exámenes de lápiz y papel, omitir el `destroy` de listas temporales no descuenta puntos para agilizar el dibujo; sin embargo, en diagramación formal se incluye para reflejar con precisión el ciclo de vida del objeto.

### 5. Listas Temporales
- **Responsabilidad única:** El objeto `Sistema` es el **único responsable** de instanciar (`create ListaTemp`) y poblar (`add(item)`) las listas temporales durante un `loop`.
- **Sin retorno desde la lista:** Como `Sistema` ya tiene la referencia, **NUNCA** graficar una flecha de retorno desde la lista temporal hacia `Sistema`.
- **Retorno al Controlador:** El retorno de la colección se grafica desde `Sistema` hacia el `CTRLCU`: `Sistema --> CTRLCU: (listaTemporal, tiempoTotal)`.
- **Cierre del ciclo:** Se puede finalizar con `destroy listaTemporal`.

### 6. Creación de Entidades de Negocio y Composición
- **Entidades Simples:** Tras un proceso exitoso, `Sistema` ejecuta el `create(...)` de la nueva entidad.
- **Estructuras Todo-Parte (Composición):** Cuando se crean cabeceras con detalle (ej. `Venta` y `LineaVenta`):
  1. `Sistema` hace el `create(...)` de la Cabecera (`Venta`).
  2. Dentro de un `loop`, la Cabecera recibe el mensaje para agregar detalles (`agregarLinea(item)`).
  3. **Es la Cabecera** (y no el Sistema) la que hace el `create(...)` hacia sus objetos hijo (`LineaVenta`).

### 7. Fragmentos de Interacción y Manejo de Errores
- **`alt` (Alternativa):** Obligatorio para flujos mutuamente excluyentes (ej. `pago aprobado` vs `pago rechazado`).
  - *Manejo de Errores:* En la rama de error de un `alt`, el flujo normal se interrumpe y se deben graficar retornos (`-->`) en cascada hacia atrás hasta la UI (ej. `Sistema --> CTRLCU: error` / `CTRLCU --> UI: mostrarError()`).
- **`opt` (Opcional):** Para validaciones condicionales simples (if sin else).
- **`loop` (Bucle):** Obligatorio al recorrer colecciones desde el `Sistema`.
- **`ref` (Referencia):** Usar para encapsular flujos complejos definidos en otros diagramas (ej. `ref over CTRLCU, Banco: Validar Pago`).

### 8. Notación de Mensajes de Retorno
- **Sin la palabra `return`:** La línea discontinua (`-->`) denota el retorno.
- **Formato estricto:** `--> CTRLCU: listaAlumnos` (solo el valor/objeto de retorno, prohibido poner `return:` o `retornar`).

### 9. Actores Externos y Sincronía
- **Mensajes Síncronos (->):** Flecha de punta rellena. Uso por defecto para toda comunicación interna del sistema (emisor espera respuesta).
- **Mensajes Asíncronos (->>):** Flecha de punta abierta. **Obligatorio** para comunicarse con **Actores Externos** (AFIP, Bancos, APIs) debido a la latencia.
- **Tratamiento:** Se modelan como `actor` a la derecha de todo. El `CTRLCU` les envía el mensaje directamente (`CTRLCU ->> Banco: validar()`). NUNCA se modelan como clases internas del sistema.

### 10. Coherencia con el Diagrama de Clases
- Todo mensaje enviado a un objeto (derecha de la flecha) debe existir con la misma firma como método en su Tarjeta CRC y Diagrama de Clases.

---

## 🏗️ Estructura Típica de Participantes (PlantUML)

Al diagramar en PlantUML, ordenar estrictamente los participantes en este orden (Izquierda a Derecha):

```plantuml
skinparam style strictuml

actor "Usuario" as Actor
boundary "PantallaCU" as UI <<Boundary>>
control "CTRLCU" as Gestor <<Control>>
control "CTRLSesion" as Sesion <<Control>>
participant "Sistema" as Sys <<Sistema / Fachada>>
collections "EntidadColeccion" as Entidades <<Entity>>
entity "listaTemporal" as TempList <<Lista Temporal>>
entity "EntidadNueva" as NuevaEnt <<Entity>>
actor "ServicioExterno" as API <<Actor Externo>>
```

---

## 🔄 Flujo de Trabajo para Generación

Cuando debas resolver un diagrama de secuencia:

1. **Autenticación:** Extraer datos del actor mediante `CTRLCU -> Sesion: obtenerUsuarioLogueado()`.
2. **Petición Principal:** `CTRLCU -> Sys: invocarOperacion(params)`.
3. **Colecciones y Listas Temporales:** 
   - `Sys -> TempList: create()`
   - `loop` iterando la colección: `Sys` hace `get()` a la entidad y luego `add()` a `TempList`.
4. **Validaciones y Reglas de Negocio:** Usar patrón experto enviando mensajes a las entidades para que calculen. Usar `alt` para errores.
5. **Creación de Entidades:** Si aplica, `Sys` hace `create()` de la entidad central.
6. **Cierre:** `Sys --> CTRLCU: resultado` y `CTRLCU --> UI: mostrarExito()`.