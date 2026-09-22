# DSI 2026 - Diseño de Sistemas (UML, CRC y Casos de Uso)

Repositorio integral de trabajo para la materia **Diseño de Sistemas (2026)**.
Contiene plantillas formales, ejemplos resueltos interconectados y automatización de renderizado vectorial **SVG** mediante **PlantUML** y scripts Bash.

---

## 🚀 Inicio Rápido: Renderizado a SVG

El script [`render.sh`](file:///home/sando/Documents/dsi-2026/render.sh) compila de forma automática cualquier diagrama `.puml` a formato vectorial `.svg` (o PNG si se prefiere).

```bash
# 1. Compilar TODOS los diagramas del proyecto a SVG (casos de uso, secuencia, clases)
./render.sh

# 2. Compilar un diagrama puntual
./render.sh clases/ejemplos/clases_gestion_pedidos.puml

# 3. Compilar todos los diagramas de una carpeta específica
./render.sh clases/

# 4. Modo Observador (Watch Mode): Re-compila automáticamente al guardar cambios
./render.sh -w

# 5. Exportar a formato PNG en lugar de SVG
./render.sh -f png secuencia/ejemplos/seq_crear_pedido.puml
```

> **Visualización rápida:** En Linux / Pop!_OS puedes abrir cualquier SVG en tu visor preferido o navegador:
> ```bash
> xdg-open clases/ejemplos/clases_gestion_pedidos.svg
> ```

---

## 📂 Estructura Completa del Proyecto

El repositorio está organizado en 5 áreas clave del diseño orientado a objetos:

```text
dsi-2026/
├── bin/
│   └── plantuml                             # Wrapper CLI ejecutable de PlantUML (motor Smetana)
├── render.sh                                # Script Bash principal para compilar diagramas a SVG
│
├── casos-de-uso/                            # 1. DIAGRAMAS DE CASOS DE USO
│   ├── plantillas/
│   │   └── plantilla_caso_uso.puml          # Plantilla UML (Actores, límites, <<include>>, <<extend>>)
│   └── ejemplos/
│       ├── cu_gestion_pedidos.puml          # Diagrama resuelto del módulo de pedidos
│       └── cu_gestion_pedidos.svg           # Gráfico SVG vectorial compilado
│
├── descripcion-casos-de-uso/                # 2. DESCRIPCIÓN Y ESPECIFICACIÓN DE CU
│   ├── plantillas/
│   │   └── plantilla_descripcion_cu.md      # Plantilla académica (Pre/Post, Camino feliz, Alternativas)
│   └── ejemplos/
│       └── descripcion_cu_crear_pedido.md   # Especificación formal paso a paso del CU-03
│
├── tarjetas-crc/                            # 3. TARJETAS CRC (Clase - Responsabilidad - Colaborador)
│   ├── plantillas/
│   │   └── plantilla_tarjeta_crc.md         # Plantilla con formato Anverso (Frente) / Reverso (Dorso)
│   └── ejemplos/
│       └── crc_gestion_pedidos.md           # Tarjetas CRC de Pedido, Detalle, Producto, Cliente, etc.
│
├── secuencia/                               # 4. DIAGRAMAS DE SECUENCIA
│   ├── plantillas/
│   │   └── plantilla_secuencia.puml         # Plantilla con ECB, CTRLSesion, autonumber y fragmentos
│   └── ejemplos/
│       ├── seq_crear_pedido.puml            # Realización técnica del CU-03
│       └── seq_crear_pedido.svg             # Gráfico SVG vectorial compilado
│
├── clases/                                  # 5. DIAGRAMAS DE CLASES
│   ├── plantillas/
│   │   └── plantilla_diagrama_clases.puml   # Plantilla UML (Entidades, Control, Límites, asociativas)
│   └── ejemplos/
│       ├── clases_gestion_pedidos.puml      # Modelo de clases del dominio y control
│       └── clases_gestion_pedidos.svg       # Gráfico SVG vectorial compilado
│
├── .agents/skills/dsi-secuencia/            # Skill de cátedra de DSI para el asistente
│   ├── SKILL.md                             # Reglas de oro y criterios de parcial
│   ├── references/pautas_catedra.md         # Fundamentos teóricos y consultas docentes
│   └── examples/ejemplo_secuencia_catedra.puml # Ejemplo que aplica las pautas de cátedra
└── README.md
```

---

## 📇 Formato de Tarjetas CRC

Cada tarjeta CRC sigue estrictamente la convención pedagógica de doble faz:

- **Anverso (Frente):**
  - **Nombre de la clase:** Identificador único de la clase.
  - **Propósito:** Descripción breve y clara del rol que cumple dentro del dominio del problema.
- **Reverso (Dorso):**
  - **Responsabilidades:** Qué tareas sabe realizar (*Hacer*) y qué información conoce (*Saber*).
  - **Colaboradores:** Otras clases con las que interactúa para cumplir sus tareas (quién le pide servicios o a quién ella debe pedir servicios).

Ver [`tarjetas-crc/plantillas/plantilla_tarjeta_crc.md`](file:///home/sando/Documents/dsi-2026/tarjetas-crc/plantillas/plantilla_tarjeta_crc.md) y [`tarjetas-crc/ejemplos/crc_gestion_pedidos.md`](file:///home/sando/Documents/dsi-2026/tarjetas-crc/ejemplos/crc_gestion_pedidos.md).

---

## 📐 Coherencia Trazable entre Modelos

Todos los ejemplos incluidos corresponden al mismo problema (**Gestión de Pedidos de Comercio Electrónico**), demostrando la trazabilidad completa requerida en Diseño de Sistemas:

$$\text{Diagrama CU} \longleftrightarrow \text{Descripción CU} \longleftrightarrow \text{Tarjetas CRC} \longleftrightarrow \text{Diagrama de Secuencia} \longleftrightarrow \text{Diagrama de Clases}$$
