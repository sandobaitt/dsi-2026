# Pseudocódigo: Realización de CU Registrar Préstamo

> Derivado del [diagrama de clases](../03-diagrama-clases/clases_registrar_prestamo.puml): una sección por clase y un `MÉTODO` por cada operación del diagrama, con la misma firma.

## Convenciones

* `fechaActual` es la fecha del sistema.
* Los roles de las asociaciones son los nombres del diagrama: `sistema`, `ui`, `ctrl`, `estudiantes`, `ejemplares`, `tiposPrestamo`, `prestamos`, `sanciones`, `detalles`, `tipo`, `ejemplar`.
* `alumnado` es el actor externo (Sistema de Alumnado): no es una clase. La consulta es asíncrona y el Sistema espera su respuesta.
* Las validaciones que originan los caminos alternativos se resuelven con `LANZAR ErrorNegocio(mensaje, retoma)`, donde `retoma` indica cómo continúa el CU: `PASO_2`, `PASO_5` o `FIN`. Cada una lleva el número del camino alternativo de la [descripción](../01-descripcion-cu/descripcion_cu_registrar_prestamo.md).
* La cancelación (7.a) no se modela: no modifica el dominio.
* UI y Controlador no tienen atributos. Los datos intermedios del CU (`e`, `idTipoPrestamo`, `ejemplares`) son variables de la ejecución que viajan entre los métodos del controlador.

---

## Capa de Presentación y Control

### Clase `PantallaRegistrarPrestamo`

```text
MÉTODO seleccionarOpcionRegistrarPrestamo()
    ctrl.iniciarRegistroPrestamo()

MÉTODO solicitarLegajo()
    MOSTRAR "Ingrese el legajo del estudiante"

MÉTODO ingresarLegajo(nroLegajo: int)
    ctrl.tomarLegajo(nroLegajo)

MÉTODO solicitarTipoYEjemplares()
    MOSTRAR "Seleccione el tipo de préstamo e ingrese los ejemplares a llevar"

MÉTODO ingresarTipoYEjemplares(idTipoPrestamo: int, codigos: List)
    ctrl.tomarTipoYEjemplares(idTipoPrestamo, codigos)

MÉTODO mostrarResumenPrestamo(ejemplares: List)
    MOSTRAR "Resumen del préstamo", ejemplares
    MOSTRAR "¿Confirma el préstamo?"

MÉTODO confirmarPrestamo()
    ctrl.tomarConfirmacion()

MÉTODO mostrarPrestamoRegistrado(p: Prestamo)
    MOSTRAR "Préstamo registrado", p con la fecha límite de cada ejemplar

MÉTODO mostrarError(mensaje: String)
    MOSTRAR mensaje
```

### Clase `CTRLRegistrarPrestamo`

```text
MÉTODO iniciarRegistroPrestamo()
    ui.solicitarLegajo()

MÉTODO tomarLegajo(nroLegajo: int)
    INTENTAR
        e ← sistema.verificarEstudiante(nroLegajo)
        ui.solicitarTipoYEjemplares()
    CAPTURAR ErrorNegocio ex                         // caminos alternativos 4.a a 4.e
        ui.mostrarError(ex.mensaje)
        SI ex.retoma = PASO_2 ENTONCES
            ui.solicitarLegajo()
        SINO
            ui.destroy()                             // fin del CU
        FIN SI

MÉTODO tomarTipoYEjemplares(idTipoPrestamo: int, codigos: List)
    INTENTAR
        ejemplares ← sistema.verificarEjemplares(e, codigos)
        ui.mostrarResumenPrestamo(ejemplares)
    CAPTURAR ErrorNegocio ex                         // caminos alternativos 6.a y 6.b
        ui.mostrarError(ex.mensaje)
        ui.solicitarTipoYEjemplares()                // retoma en el paso 5

MÉTODO tomarConfirmacion()
    p ← sistema.registrarPrestamo(e, idTipoPrestamo, ejemplares)
    ui.mostrarPrestamoRegistrado(p)
    ui.destroy()                                     // fin del CU: el controlador destruye la UI
```

### Clase `Sistema`

```text
MÉTODO verificarEstudiante(nroLegajo: int): Estudiante
    e ← BUSCAR en estudiantes el estudiante CON nroLegajo
    SI e ES NULO ENTONCES
        LANZAR ErrorNegocio("El legajo no corresponde a un estudiante", PASO_2)               // 4.a
    FIN SI

    esRegular ← alumnado.consultarEstudianteRegular(nroLegajo)
    SI alumnado NO RESPONDE ENTONCES
        LANZAR ErrorNegocio("No se pudo obtener la situación académica", FIN)                 // 4.e
    FIN SI
    SI NO esRegular ENTONCES
        LANZAR ErrorNegocio("El estudiante no es regular", FIN)                               // 4.b
    FIN SI

    SI e.tieneLibrosVencidos(fechaActual) ENTONCES
        LANZAR ErrorNegocio("El estudiante tiene libros vencidos sin devolver", FIN)          // 4.c
    FIN SI
    SI e.tieneSancionVigente(fechaActual) ENTONCES
        LANZAR ErrorNegocio("El estudiante se encuentra sancionado", FIN)                     // 4.d
    FIN SI

    RETORNAR e

MÉTODO verificarEjemplares(e: Estudiante, codigos: List): List
    SI NO e.puedeLlevar(CANTIDAD(codigos)) ENTONCES
        LANZAR ErrorNegocio("Supera el máximo de tres ejemplares prestados", PASO_5)          // 6.b
    FIN SI

    LTTEJ ← NUEVA lista temporal de Ejemplar
    noDisponibles ← NUEVA lista de códigos
    PARA CADA codigo EN codigos HACER
        ej ← BUSCAR en ejemplares el ejemplar CON codigoInventario = codigo
        SI ej NO ES NULO Y ej.estaDisponible() ENTONCES
            LTTEJ.add(ej)
        SINO
            noDisponibles.add(codigo)
        FIN SI
    FIN PARA

    SI noDisponibles NO ESTÁ VACÍA ENTONCES
        DESTRUIR LTTEJ
        LANZAR ErrorNegocio("Ejemplares no disponibles: " + noDisponibles, PASO_5)            // 6.a
    FIN SI

    RETORNAR LTTEJ

MÉTODO registrarPrestamo(e: Estudiante, idTipoPrestamo: int, ejemplares: List): Prestamo
    tp ← BUSCAR en tiposPrestamo el tipo CON idTipoPrestamo
    fechaLimite ← tp.calcularFechaLimite(fechaActual)

    p ← NUEVO Prestamo(e, tp, fechaActual)
    e.agregarPrestamo(p)

    PARA CADA ej EN ejemplares HACER
        p.agregarDetalle(ej, fechaLimite)
        ej.prestar()
    FIN PARA

    DESTRUIR ejemplares                                // lista temporal ya utilizada
    RETORNAR p
```

---

## Capa de Dominio

### Clase `Estudiante`

```text
CONSTANTE LIMITE_EJEMPLARES = 3

MÉTODO tieneLibrosVencidos(fechaActual: Date): boolean
    PARA CADA p EN prestamos HACER
        SI p.tieneVencidos(fechaActual) ENTONCES
            RETORNAR verdadero
        FIN SI
    FIN PARA
    RETORNAR falso

MÉTODO tieneSancionVigente(fechaActual: Date): boolean
    PARA CADA s EN sanciones HACER
        SI s.estaVigente(fechaActual) ENTONCES
            RETORNAR verdadero
        FIN SI
    FIN PARA
    RETORNAR falso

MÉTODO puedeLlevar(cantidad: int): boolean
    prestados ← 0
    PARA CADA p EN prestamos HACER
        prestados ← prestados + p.cantidadPendientes()
    FIN PARA
    RETORNAR prestados + cantidad <= LIMITE_EJEMPLARES

MÉTODO agregarPrestamo(p: Prestamo)
    prestamos.add(p)
```

### Clase `Sancion`

```text
MÉTODO estaVigente(fechaActual: Date): boolean
    RETORNAR fechaInicio <= fechaActual Y fechaActual <= fechaFin
```

### Clase `Prestamo`

```text
CONSTRUCTOR Prestamo(e: Estudiante, tp: TipoPrestamo, fechaPrestamo: Date)
    estudiante ← e
    tipo ← tp
    detalles ← NUEVA lista vacía
    // nroPrestamo lo asigna el sistema al crearlo

MÉTODO agregarDetalle(ejemplar: Ejemplar, fechaLimite: Date)
    d ← NUEVO DetallePrestamo(ejemplar, fechaLimite)       // la cabecera crea sus detalles
    detalles.add(d)

MÉTODO tieneVencidos(fechaActual: Date): boolean
    PARA CADA d EN detalles HACER
        SI d.estaVencido(fechaActual) ENTONCES
            RETORNAR verdadero
        FIN SI
    FIN PARA
    RETORNAR falso

MÉTODO cantidadPendientes(): int
    cantidad ← 0
    PARA CADA d EN detalles HACER
        SI d.estaPendiente() ENTONCES
            cantidad ← cantidad + 1
        FIN SI
    FIN PARA
    RETORNAR cantidad
```

### Clase `DetallePrestamo`

```text
CONSTRUCTOR DetallePrestamo(ejemplar: Ejemplar, fechaLimite: Date)
    ejemplar ← ejemplar
    fechaLimite ← fechaLimite
    fechaDevolucion ← NULA                                  // mientras no se devuelve

MÉTODO estaPendiente(): boolean
    RETORNAR fechaDevolucion ES NULA

MÉTODO estaVencido(fechaActual: Date): boolean
    RETORNAR estaPendiente() Y fechaLimite < fechaActual
```

### Clase `TipoPrestamo`

```text
MÉTODO calcularFechaLimite(fechaPrestamo: Date): Date
    RETORNAR fechaPrestamo + diasPlazo DÍAS                 // a domicilio: 7; en sala: 0
```

### Clase `Ejemplar`

```text
MÉTODO estaDisponible(): boolean
    RETORNAR estado = "Disponible"

MÉTODO prestar()
    estado ← "Prestado"
```
