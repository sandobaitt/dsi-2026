# DSI 2026 - Diseño de Sistemas (UML, CRC y Casos de Uso)

Repositorio integral de trabajo y preparación para el parcial de **Diseño de Sistemas (2026)**, estructurado con base en las directrices de cátedra del **[Apunte Parcial Práctico 2026](file:///home/sando/Documents/dsi-2026/apunte.md)**.

---

## 🎯 Metodología de Resolución de Examen

Siguiendo el orden oficial de resolución establecido por la cátedra:

```text
1. Diagrama de Casos de Uso (Delimitación del sistema y actores)
   ↓
2. Descripción de Caso de Uso (Especificación textual paso a paso del CU)
   ↓
3. Realización del Caso de Uso (Para el CU seleccionado exclusivamente):
   ├── 3.1. Diagrama de Clases (Únicamente clases del CU; UI y CTRL sin atributos)
   ├── 3.2. Tarjetas CRC (Anverso/Reverso para cada clase del diagrama)
   └── 3.3. Diagrama de Secuencia (Flujo temporal con CTRLSesion y pautas de cátedra)
```

---

## 🚀 Compilación y Renderizado a SVG

El script [`render.sh`](file:///home/sando/Documents/dsi-2026/render.sh) compila de forma automática cualquier diagrama `.puml` a formato vectorial `.svg`:

```bash
# Compilar TODOS los diagramas del proyecto a SVG
./render.sh

# Compilar una carpeta específica
./render.sh casos-de-uso/
./render.sh clases/
./render.sh secuencia/

# Compilar un archivo puntual
./render.sh clases/ejemplos/clases_gestion_pedidos.puml

# Modo Observador (Watch Mode en tiempo real)
./render.sh -w
```

---

## 📂 Organización de Carpetas y Artefactos

```text
dsi-2026/
├── apunte.md                                # Apunte oficial de cátedra (Mateo Lopez, Agustin Carrasco, Lautaro Sandoval)
├── bin/plantuml                             # Wrapper CLI ejecutable de PlantUML con motor Smetana
├── render.sh                                # Script de automatización Bash para generar SVG
│
├── casos-de-uso/                            # 1. DIAGRAMAS DE CASOS DE USO
│   ├── plantillas/plantilla_caso_uso.puml   # Plantilla (asociaciones sin flechas, límites del sistema)
│   └── ejemplos/cu_gestion_pedidos.puml     # Ejemplo resuelto (include, extend, herencia)
│
├── descripcion-casos-de-uso/                # 2. DESCRIPCIÓN TEXTUAL DE CASOS DE USO
│   ├── plantillas/plantilla_descripcion_cu.md # Plantilla (pre/post al sistema, alternancia 1 a 1, sin UI)
│   └── ejemplos/descripcion_cu_crear_pedido.md # Ejemplo del CU-03 (caminos alternativos 2.a, 5.a, 5.b)
│
├── clases/                                  # 3.1. DIAGRAMAS DE CLASES DE LA REALIZACIÓN
│   ├── plantillas/plantilla_diagrama_clases.puml # Plantilla (UI y CTRL SIN atributos; clases transaccionales)
│   └── ejemplos/clases_gestion_pedidos.puml # Modelo de clases coherente con secuencia y CRC
│
├── tarjetas-crc/                            # 3.2. TARJETAS CRC
│   ├── plantillas/plantilla_tarjeta_crc.md  # Plantilla formal: Anverso (Frente) / Reverso (Dorso)
│   └── ejemplos/crc_gestion_pedidos.md      # Tarjetas de las 9 clases involucradas en el CU-03
│
├── secuencia/                               # 3.3. DIAGRAMAS DE SECUENCIA
│   ├── plantillas/plantilla_secuencia.puml  # Plantilla (autonumber, fragmentos, activaciones)
│   └── ejemplos/seq_crear_pedido.puml       # Realización temporal (CTRLSesion, obtener vs get, sin return)
│
└── .agents/skills/                          # SKILLS PERSONALIZADAS PARA CADA ARTEFACTO
    ├── dsi-casos-de-uso/SKILL.md            # Reglas para DCU (asociaciones sin flechas, roles, include/extend)
    ├── dsi-descripcion-cu/SKILL.md          # Reglas para descripción (lenguaje generalista, no repetir precondición)
    ├── dsi-clases/SKILL.md                  # Reglas para clases (UI y CTRL sin atributos, correspondencia con métodos)
    ├── dsi-tarjetas-crc/SKILL.md            # Reglas para CRC (formato Frente/Dorso, coherencia 1 a 1 con clases)
    └── dsi-secuencia/SKILL.md               # Reglas para secuencia (CTRLSesion, listas temporales por Sistema)
```

---

## 📐 Coherencia Total del Problema de Ejemplo

El ejemplo transversal del repositorio modela la **Gestión y Creación de Pedidos (CU-03)** en todos los modelos:

1. **Diagrama de CU:** `Cliente -- CU_CrearPedido ..> CU_ProcesarPago : <<include>>`.
2. **Descripción de CU:** Paso a paso alternado Actor-Sistema, sin mencionar botones ni tecnologías.
3. **Diagrama de Clases:** `UI` y `ControladorPedido` sin atributos; `Pedido` como clase transaccional que relaciona `Cliente`, `Producto` y `Factura`.
4. **Tarjetas CRC:** 9 tarjetas con Anverso (Clase, Propósito) y Reverso (Responsabilidades: Hacer/Saber, Colaboración).
5. **Diagrama de Secuencia:** `ControladorPedido` consulta credenciales a `CTRLSesion` preexistente; `Sistema` crea la lista temporal y decrementa stock; sin `destroy` y sin la palabra `return`.
