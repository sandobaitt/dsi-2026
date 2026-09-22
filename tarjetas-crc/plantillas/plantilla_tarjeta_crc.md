# Plantilla: Tarjetas CRC (Cátedra DSI 2026)

> **Pauta de Cátedra:**
> - **Coherencia absoluta:** Toda clase presente en el Diagrama de Clases de la realización debe tener su tarjeta CRC.
> - **Foco en el CU:** Listar únicamente las responsabilidades y colaboraciones que la clase ejecuta para resolver el caso de uso analizado.
> - **Formato:** Dos caras bien diferenciadas: **Anverso (Frente)** y **Reverso (Dorso)**.

---

## Formato Estándar de Tarjeta CRC

### Tarjeta CRC: `[NombreDeLaClase]`

#### 📇 Anverso (Frente)

```text
+-----------------------------------------------------------+
| Clase: [Nombre de la Clase]                               |
| Propósito: [Breve descripción de su rol en el sistema]    |
+-----------------------------------------------------------+
```

* **Clase:** `[Nombre de la Clase]`
* **Propósito:** [Descripción breve y clara del rol que cumple la clase dentro del dominio del problema.]

---

#### 🔄 Reverso (Dorso)

```text
+-----------------------------+-----------------------------+
| Responsabilidades:          | Colaboración:               |
+-----------------------------+-----------------------------+
| - [Tarea que realiza]       | - [Clase colaboradora]      |
| - [Información que conoce]  |                             |
+-----------------------------+-----------------------------+
```

| Responsabilidades | Colaboración |
| :--- | :--- |
| **Hacer (Tareas que realiza en el CU):**<br>- [Método / Tarea clave 1]<br>- [Método / Tarea clave 2]<br><br>**Saber (Información que conoce en el CU):**<br>- [Dato o atributo relevante 1]<br>- [Dato o atributo relevante 2] | - **[Clase Colaboradora A]:** [Motivo de la colaboración, ej: solicita validación de...]<br>- **[Clase Colaboradora B]:** [ej: provee datos para...] |
