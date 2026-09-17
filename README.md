# DSI 2026 - Diseño de Sistemas (UML con PlantUML)

Repositorio de trabajo para la materia **Diseño de Sistemas (2026)**.
Incluye entorno automatizado para diagramado con **PlantUML**, generación de gráficos vectoriales **SVG** en tiempo real, plantillas y especificaciones formales de **Casos de Uso** y **Diagramas de Secuencia**.

---

## 🚀 Inicio Rápido: Renderizado a SVG

El script [`render.sh`](file:///home/sando/Documents/dsi-2026/render.sh) compila de forma automática cualquier diagrama `.puml` a formato vectorial `.svg` (o PNG si se prefiere).

### Comandos más usados

```bash
# 1. Compilar TODOS los diagramas del proyecto a SVG
./render.sh

# 2. Compilar un diagrama específico
./render.sh secuencia/ejemplos/seq_crear_pedido.puml

# 3. Compilar todos los diagramas de una carpeta
./render.sh casos-de-uso/

# 4. Modo Observador (Watch Mode): Re-compila automáticamente al guardar cambios
./render.sh -w

# 5. Exportar a formato PNG en lugar de SVG
./render.sh -f png secuencia/ejemplos/seq_crear_pedido.puml
```

> **Consejo para visualización:** En Linux / Pop!_OS puedes abrir cualquier SVG en tu navegador o visor de imágenes ejecutando:
> ```bash
> xdg-open casos-de-uso/ejemplos/cu_gestion_pedidos.svg
> ```
> O previsualizarlo directamente en el IDE.

---

## 📂 Estructura del Repositorio

```text
dsi-2026/
├── bin/
│   └── plantuml                     # Wrapper ejecutable de PlantUML con motor Smetana integrado
├── render.sh                        # Script Bash principal para renderizar a SVG
├── casos-de-uso/
│   ├── plantillas/
│   │   ├── plantilla_caso_uso.puml        # Plantilla PlantUML para Diagramas de Casos de Uso
│   │   └── plantilla_especificacion_cu.md # Plantilla formal para especificaciones de Casos de Uso
│   └── ejemplos/
│       ├── cu_gestion_pedidos.puml        # Ejemplo: Diagrama de Casos de Uso (incluye <<include>>, <<extend>>)
│       ├── cu_gestion_pedidos.svg         # SVG generado automáticamente
│       └── especificacion_cu_crear_pedido.md # Especificación formal paso a paso del CU-03
├── secuencia/
│   ├── plantillas/
│   │   └── plantilla_secuencia.puml       # Plantilla con arquitectura ECB, autonumber y fragmentos
│   └── ejemplos/
│       ├── seq_crear_pedido.puml          # Realización del CU-03 Crear Pedido en diagrama de secuencia
│       └── seq_crear_pedido.svg           # SVG generado automáticamente
└── README.md
```

---

## 📐 Guía de Diagramas y Buenas Prácticas

### 1. Diagramas de Casos de Uso
- **Actores:** Nombrados con roles claros en singular (*Cliente*, *Cajero*).
  - Actores primarios a la izquierda (inician el valor).
  - Actores secundarios o externos a la derecha (servicios de apoyo).
- **Casos de Uso:** Nombrados siempre con **Verbo en infinitivo + Sustantivo** (ej: *Registrar Cliente*, *Emitir Factura*).
- **Relaciones:**
  - `<<include>>`: El caso de uso base siempre delega o requiere la ejecución del CU incluido (la flecha punteada apunta hacia el caso incluido).
  - `<<extend>>`: Comportamiento opcional o condicional bajo un punto de extensión (la flecha apunta hacia el caso base).

### 2. Especificación Textual de Casos de Uso
- Usar la plantilla en [`casos-de-uso/plantillas/plantilla_especificacion_cu.md`](file:///home/sando/Documents/dsi-2026/casos-de-uso/plantillas/plantilla_especificacion_cu.md).
- Detallar claramente **Precondiciones**, **Garantías de Éxito**, **Flujo Principal (Paso a paso Actor vs Sistema)** y **Flujos Alternativos / Excepcionales**.

### 3. Diagramas de Secuencia
- Usar la plantilla en [`secuencia/plantillas/plantilla_secuencia.puml`](file:///home/sando/Documents/dsi-2026/secuencia/plantillas/plantilla_secuencia.puml).
- Estructuración recomendada: **Arquitectura en Capas o ECB (Entity - Control - Boundary)**.
- Activar y desactivar líneas de vida (`activate`, `deactivate`).
- Emplear fragmentos combinados estándar:
  - `alt` / `else`: Alternativas condicionales (ej: pago aprobado vs rechazado).
  - `opt`: Acciones opcionales.
  - `loop`: Bucles de procesamiento (ej: recorrer ítems de un pedido).

---

## 🛠️ Configuración Interna

- **PlantUML JAR:** `~/.local/share/plantuml/plantuml.jar` (v1.2026.8).
- **Motor de Maquetado:** Smetana (incorporado en Java puro, no requiere dependencias de sistema adicionales).
- **CLI Global:** Disponible en terminal mediante el comando `plantuml`.
