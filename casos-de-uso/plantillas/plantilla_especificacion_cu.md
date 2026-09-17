# Plantilla de Especificación de Caso de Uso

## Información General

| Campo | Detalle |
| :--- | :--- |
| **Identificador:** | `CU-XX` |
| **Nombre del Caso de Uso:** | Verbo en infinitivo + Sustantivo (ej: *Registrar Pedido*) |
| **Autor / Diseñador:** | Nombre del estudiante / diseñador |
| **Versión / Fecha:** | `v1.0 - DD/MM/AAAA` |
| **Complejidad:** | Baja / Media / Alta |

---

## 1. Descripción / Propósito
Breve párrafo que describe el objetivo de valor que el actor persigue al ejecutar este caso de uso dentro del sistema.

---

## 2. Actores
- **Actor Primario:** Quien inicia y participa activamente en el caso de uso (ej: *Cliente*, *Cajero*).
- **Actor Secundario:** Participante de soporte o sistema externo (ej: *Pasarela de Pagos*, *Servicio de Facturación*).

---

## 3. Precondiciones
Condiciones que el sistema y el entorno deben cumplir **antes** de que comience el caso de uso.
1. El actor debe estar autenticado en el sistema con el rol correspondiente.
2. El catálogo de productos debe estar cargado y con stock disponible.

---

## 4. Postcondiciones (Garantías)
- **Garantía de Éxito (Postcondición Exitosa):** Estado del sistema una vez finalizado el flujo con éxito (ej: *El pedido queda registrado en estado 'Confirmado' y el stock es decrementado*).
- **Garantía Mínima (En caso de fallo):** Estado del sistema si el caso de uso es cancelado o abortado (ej: *No se descuenta dinero del cliente ni se alteran las reservas de stock*).

---

## 5. Disparador (Trigger)
Evento externo que inicia la ejecución del caso de uso (ej: *El cliente presiona el botón 'Confirmar compra'*).

---

## 6. Flujo Principal de Eventos (Camino Feliz)

| Paso | Acción del Actor | Reacción / Proceso del Sistema |
| :---: | :--- | :--- |
| **1** | El actor solicita iniciar la operación... | |
| **2** | | El sistema solicita los datos requeridos (formulario de...). |
| **3** | El actor ingresa los datos y confirma. | |
| **4** | | El sistema valida los datos ingresados según las reglas RN-01. |
| **5** | | El sistema registra la información y persiste los cambios. |
| **6** | | El sistema emite un comprobante y confirma la operación al actor. |

---

## 7. Flujos Alternativos y Excepcionales

### Flujo Alternativo 4a: Datos Inválidos o Incompletos
- **Condición:** Los datos ingresados no cumplen con el formato o campos requeridos.
- **Acciones:**
  1. El sistema informa el error puntual en el campo correspondiente.
  2. El sistema mantiene los datos válidos precargados.
  3. El flujo regresa al **Paso 3** del Flujo Principal.

### Flujo Excepcional 4b: Stock Insuficiente
- **Condición:** Al verificar disponibilidad, no hay stock para uno de los ítems.
- **Acciones:**
  1. El sistema notifica al actor los ítems agotados.
  2. El actor decide si ajusta la cantidad o cancela.
  3. Si cancela, el caso de uso finaliza cumpliendo la Garantía Mínima.

---

## 8. Puntos de Extensión
- **PE-01 (Aplicar Cupón de Descuento):** En el Paso 3, si el cliente posee un código promocional, se extiende al `CU-08: Validar Cupón Promocional`.

---

## 9. Reglas de Negocio Asociadas
- **RN-01:** Monto mínimo de compra para envío gratis ($X).
- **RN-02:** Política de reintento de cobro ante timeout.
