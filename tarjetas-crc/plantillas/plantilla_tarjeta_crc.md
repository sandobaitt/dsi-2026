# Plantilla: Tarjetas CRC (Clase - Responsabilidad - Colaborador)

Las tarjetas CRC son una técnica de diseño orientada a objetos para definir el rol, las responsabilidades (lo que la clase *sabe* y lo que *hace*) y los colaboradores de cada clase del dominio y del sistema.

---

## Formato Estándar de Tarjeta CRC

```markdown
┌────────────────────────────────────────────────────────────────────────┐
│                          ANVERSO (FRENTE)                              │
├────────────────────────────────────────────────────────────────────────┤
│ Nombre de la clase: [Identificador único de la clase]                  │
│ Propósito: [Descripción breve y clara del rol que cumple la clase      │
│            dentro del dominio del problema]                            │
└────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│                          REVERSO (DORSO)                               │
├───────────────────────────────────┬────────────────────────────────────┤
│ Responsabilidades                 │ Colaboradores                      │
├───────────────────────────────────┼────────────────────────────────────┤
│ [Qué tareas sabe realizar y qué   │ [Otras clases con las que          │
│  información conoce: métodos      │  interactúa para cumplir sus       │
│  principales y datos clave]       │  tareas: quién le pide servicios o │
│                                   │  a quién ella debe pedir servicios]│
└───────────────────────────────────┴────────────────────────────────────┘
```

---

## Plantilla Markdown Reutilizable

Copia y pega este bloque para cada clase que necesites modelar:

### Tarjeta CRC: `[NombreDeLaClase]`

#### 📇 Anverso (Frente)
- **Nombre de la clase:** `[Identificador único de la clase]`
- **Propósito:** [Descripción breve y clara del rol que cumple la clase dentro del dominio del problema.]

#### 🔄 Reverso (Dorso)

| Responsabilidades | Colaboradores |
| :--- | :--- |
| **Saber (Información que conoce):**<br>- [Dato 1: ej. identificador único, estado]<br>- [Dato 2: ej. fecha, monto total]<br><br>**Hacer (Tareas que sabe realizar):**<br>- [Tarea 1: método principal o cálculo]<br>- [Tarea 2: validación de reglas de negocio]<br>- [Tarea 3: actualización de estado] | - **[Colaborador A]:** [Rol de la colaboración, ej. le provee datos de...] <br>- **[Colaborador B]:** [ej. a quién le solicita el cálculo de...] <br>- **[Colaborador C]:** [ej. clase que la invoca para...] |
