---
name: dsi-secuencia
description: >-
  Guía y pautas obligatorias de la cátedra para la resolución y diagramado de Diagramas de Secuencia
  y su coherencia con Diagramas de Clases en Diseño de Sistemas (DSI). Aplica reglas estrictas de
  CTRLSesion preexistente, no ingreso manual de datos conocidos, creación de listas temporales por Sistema,
  nomenclatura de mensajes (obtener vs get), retornos implícitos y optimizaciones para parciales.
---

# Skill: Diagramas de Secuencia - Pautas de Cátedra (DSI)

Esta skill define las reglas obligatorias, criterios de corrección y convenciones específicas de la cátedra de **Diseño de Sistemas (DSI)** para modelar **Diagramas de Secuencia** en PlantUML y asegurar su consistencia con el Diagrama de Clases.

---

## 📌 Reglas de Oro de la Cátedra (Checklist de Examen)

### 1. Manejo de Sesión y Credenciales (`CTRLSesion`)
- **Prohibido solicitar datos que el sistema ya conoce:** Nunca pedirle al actor que ingrese manualmente información de su propia sesión activa (ej: `legajo`, `idUsuario`, `rol`, `sucursal`).
- **Instancia preexistente:** El objeto de sesión activa (`CTRLSesion`) debe modelarse como un objeto **preexistente / externo** (no se instancia con `create` durante el flujo principal).
- **Ubicación en el diagrama:** Se posiciona visualmente entre el Controlador del Caso de Uso (`CTRLCU`) y la Fachada del Sistema (`Sistema`):
  $$\text{Actor} \longrightarrow \text{Pantalla (Boundary)} \longrightarrow \text{CTRLCU} \longrightarrow \text{CTRLSesion} \longrightarrow \text{Sistema} \longrightarrow \text{Entidades/Colecciones}$$
- **Comunicación:** El `CTRLCU` consulta a `CTRLSesion` para recuperar las credenciales del usuario activo (ej: `obtenerUsuarioLogueado()`, `obtenerLegajo()`).

### 2. Nomenclatura Estricta de Mensajes (`obtener` vs `get`)
- **`CTRL` $\rightarrow$ `Sistema`:** Usar verbos de negocio en español como `obtener...()`, `buscar...()`, `calcular...()` (ej: `obtenerEmpleadosDisponibles()`, `obtenerDatosUsuario()`). **Nunca usar `get` a este nivel**.
- **`Sistema` $\rightarrow$ `Entidad`:** Usar getters formales de orientación a objetos para consultar atributos de estado (ej: `getNombre()`, `getLegajo()`, `getEstado()`, `getFecha()`).

### 3. Listas Temporales y Rol de `Sistema`
- **Responsabilidad única de creación:** El objeto `Sistema` es el **único responsable** de instanciar y poblar las listas temporales (ej: `create` sobre `listaDTO` o `listaTemp`).
- **Sin retorno desde la lista temporal:** Como `Sistema` crea la lista y le añade elementos (`add()`), ya mantiene su referencia en memoria. **NO debe graficarse una flecha de retorno desde la lista hacia `Sistema`**.
- **Retorno al Controlador / Pantalla:** El retorno de la colección completa se grafica desde `Sistema` hacia el `CTRLCU` (o hacia la `UI` mediante la capa de control):
  ```text
  Sistema --> CTRLCU: listaResultado
  ```
- **Sin `destroy` para listas temporales:** No modelar llamadas `destroy` al terminar el uso de listas temporales (en parciales no resta puntos y evita sobrecargar el diagrama).

### 4. Notación de Mensajes de Retorno
- **Sin la palabra `return`:** La línea discontinua (`-->`) ya denota un mensaje de respuesta/retorno en UML.
- Formato correcto: `--> CTRLCU: listaAlumnos` (solo el valor o dato de retorno, sin `return:` ni `retornar`).

### 5. Coherencia con el Diagrama de Clases
- En el diagrama de clases debe existir una clase que **relacione a las clases del sistema** involucradas en el caso de uso (clase asociativa, transacción o entidad centralizadora de negocio).
- Todos los métodos invocados en el diagrama de secuencia deben figurar con la misma firma en las clases correspondientes.

### 6. Servicios Externos como Actores Externos
- **Tratamiento como Actor:** Cuando el sistema interactúa con un servicio o entidad externa (ej. *Entidad Bancaria* para validar pago, *AFIP* para facturación electrónica, *Proveedor de Envíos*), se modela como un **`actor` (Actor Externo)**, ubicado hacia el lado derecho del diagrama, y **no** como una clase o adaptador interno.
- **Interacción:** El objeto que coordina (ej. `CTRLCU`) le envía el mensaje directamente al actor externo (ej: `CTRLCU -> Banco: validarPago(...)`) y el actor externo responde con la línea discontinua de retorno (`Banco --> CTRLCU: pagoAprobado`).

---

## 🏗️ Estructura Típica de Participantes (Izquierda a Derecha)

Al diagramar en PlantUML, ordenar siempre los participantes en este orden:

```plantuml
actor "Usuario" as Actor
boundary "PantallaCU" as UI <<Boundary>>
control "CTRLCU" as Gestor <<Control>>
control "CTRLSesion" as Sesion <<Control>>
participant "Sistema" as Sys <<Sistema / Fachada>>
collections "EntidadColeccion" as Entidades <<Entity>>
entity "listaTemporal" as TempList <<Lista Temporal>>
```

---

## 🔄 Flujo de Trabajo para Resolver Ejercicios de Secuencia

Cuando el usuario plantee un ejercicio o caso de uso:

1. **Identificar Actores y Precondiciones de Sesión:**
   - Detectar si el usuario ya está autenticado.
   - Extraer datos del actor mediante `CTRLCU -> CTRLSesion: obtenerDatosSesion()`.
2. **Modelar Interacción con la Fachada (`Sistema`):**
   - Invocar operaciones de negocio desde `CTRLCU` a `Sistema` usando `obtener...()`.
3. **Poblado de Colecciones y Consultas a Entidades:**
   - `Sistema` crea la lista temporal (`create`).
   - `Sistema` itera sobre las entidades con `loop`.
   - `Sistema` consulta atributos con `get...()`.
   - `Sistema` agrega los datos a la lista temporal (`add()`).
4. **Cierre y Retorno:**
   - Retorno desde `Sistema` hacia `CTRLCU` con línea punteada (`-->`).
   - Cierre del flujo hacia `UI` y `Actor`.
5. **Renderizado Automático:**
   - Guardar el archivo en `secuencia/` y ejecutar `./render.sh <archivo.puml>` para validar sintaxis y generar el SVG.

---

## 📚 Referencias Adicionales

- Consultar [Pautas Detalladas de la Cátedra e Inconsistencias a Consultar](./references/pautas_catedra.md) para el análisis teórico y las preguntas sugeridas para el docente.
- Ver [Ejemplo Completo Resuelto](./examples/ejemplo_secuencia_catedra.puml) para un diagrama de referencia 100% testeado.
