# Plantilla: Descripción Formal de Caso de Uso

## 1. Ficha del Caso de Uso

| Atributo | Detalle |
| :--- | :--- |
| **Identificador:** | `CU-XX` |
| **Nombre del Caso de Uso:** | Verbo en infinitivo + Objeto / Sustantivo (ej. *Registrar Pedido*) |
| **Autor(es):** | Nombre del estudiante / equipo |
| **Versión / Fecha:** | `v1.0` - `DD/MM/AAAA` |
| **Módulo / Paquete:** | Nombre del subsistema o área funcional |
| **Complejidad:** | Baja / Media / Alta |

---

## 2. Propósito / Objetivo
Descripción breve, precisa y de alto nivel del valor de negocio que este caso de uso le entrega al actor al finalizar con éxito.

---

## 3. Actores Involucrados
- **Actor Primario:** Quien inicia la interacción y persigue el objetivo directo (ej: *Cliente*, *Médico*).
- **Actor Secundario:** Participante de soporte, receptor de notificaciones o servicio externo (ej: *Pasarela de Pagos*, *Servicio AFIP*).

---

## 4. Precondiciones
Estados o requisitos obligatorios que el sistema y su entorno deben cumplir **antes** de comenzar la ejecución:
1. El usuario debe estar autenticado en el sistema (`CTRLSesion` activa).
2. Los datos base o registros de apoyo deben existir en el sistema.

---

## 5. Postcondiciones (Garantías)
- **Garantía de Éxito (Postcondición Exitosa):** Estado final del sistema cuando el flujo concluye satisfactoriamente (ej: *El pedido queda registrado con estado 'Confirmado' y el stock se reduce*).
- **Garantía Mínima (En caso de fallo/cancelación):** Estado del sistema si la operación es abortada, falla o se cancela (ej: *No se efectúa cobro alguno ni se altera el inventario*).

---

## 6. Disparador (Trigger)
Evento exacto del actor o del sistema que inicia este caso de uso (ej: *El usuario presiona el botón 'Confirmar Solicitud'*).

---

## 7. Flujo Principal de Eventos (Camino Feliz)

| Paso | Acción del Actor | Reacción / Proceso del Sistema |
| :---: | :--- | :--- |
| **1** | El actor solicita iniciar la operación... | |
| **2** | | El sistema solicita los datos requeridos para la operación. |
| **3** | El actor ingresa los datos solicitados y confirma. | |
| **4** | | El sistema valida los datos de entrada según las reglas RN-01. |
| **5** | | El sistema procesa la transacción y registra los cambios. |
| **6** | | El sistema emite un comprobante y confirma el resultado exitoso. |

---

## 8. Flujos Alternativos y Excepcionales

### Flujo Alternativo 4a: Datos Inválidos o Incompletos
- **Condición:** Al menos un dato obligatorio no cumple el formato esperado.
- **Acciones:**
  1. El sistema resalta el campo con error y notifica el motivo.
  2. El sistema conserva los datos válidos ingresados.
  3. El flujo regresa al **Paso 3** del Flujo Principal.

### Flujo Excepcional 5a: Falla de Conexión con Servicio Externo
- **Condición:** El servicio externo responde con timeout o error 500.
- **Acciones:**
  1. El sistema reintenta la operación según política de reintentos.
  2. Si persiste el fallo, cancela la transacción garantizando la Garantía Mínima.
  3. El sistema notifica al actor y finaliza el caso de uso.

---

## 9. Puntos de Extensión
- **PE-01 (Nombre de la extensión):** En el Paso X, si ocurre la condición Y, se extiende a `CU-YY: Nombre del Caso de Uso Extensión`.

---

## 10. Reglas de Negocio Asociadas
- **RN-01:** Descripción formal de la regla de cálculo, validación o política institucional aplicable.
