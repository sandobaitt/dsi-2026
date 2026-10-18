---
name: dsi-clases-java
description: >-
  Guía de la cátedra (extra) para pasar un diagrama de clases UML a código Java en Diseño de Sistemas (DSI 2026):
  estructura general de la clase, atributos privados, constructor, visibilidad (+ - #), tabla de relaciones
  (dependencia, asociación, agregación, composición, herencia, realización) y cómo implementar las multiplicidades
  (1, 0..1, 0..*, 1..*, n..m).
---

# Skill: Diagrama de clases en Java - Pautas de Cátedra (DSI 2026)

- **Fuente de verdad:** [apunte.md](../../../apunte.md) → sección *Diagrama de clases en Java*.
- Los estereotipos (`entity`, `control`, `boundary`) **no existen en Java**: se escriben como **comentario** sobre la clase.

## Paso a paso (clase del diagrama → Java)

1. Declarar la clase con el nombre del diagrama (`public class`). Abstracta → `abstract class`; interfaz → `interface`.
2. Atributos del compartimento del medio, **siempre privados**: `private Tipo nombre;`.
3. Constructor que recibe los valores iniciales y los asigna con `this`.
4. Cada operación con su visibilidad: `+` → `public`, `-` → `private`, `#` → `protected`. Firma: `visibilidad Retorno nombre(Tipo parametro)`.
5. Por cada flecha, agregar la relación (tabla).
6. Por cada multiplicidad, elegir cómo se guarda: un objeto o una colección (`List`).
7. Revisar que cada mensaje del diagrama de secuencia exista como método en la clase que lo recibe.

## Relaciones

| Tipo | Java | Criterio |
| :--- | :--- | :--- |
| Dependencia | `void metodo(B b)` o variable local `B b` | Usa `B` como parámetro o variable local |
| Asociación | En `A`: `private B b;` | `A` conoce a `B`; la flecha indica navegabilidad |
| Agregación | En `A`: `private B b;` y `B` llega de afuera (constructor o setter `this.b = b;`) | "Tiene un"; `B` existe sin `A` |
| Composición | En `A`: `private B b = new B();` (`A` crea a `B`, nadie más lo referencia) | "Es parte de"; `B` muere con `A` |
| Generalización | `class B extends A` | "es un" |
| Realización | `class B implements A` | `B` implementa la interfaz `A` |

## Multiplicidades

| Notación | Implementación |
| :--- | :--- |
| `1` | `private Alumno alumno;` siempre asignado en el constructor |
| `0..1` | `private Prestamo prestamo;` puede ser `null` |
| `*` / `0..*` | `private List<Prestamo> prestamos = new ArrayList<>();` |
| `1..*` | `List<Ejemplar>` inicializada con al menos un elemento en el constructor |
| `n..m` | `List<...>` y el límite se valida en el método que agrega (ej: máximo 3 → `IllegalStateException`) |

## Ejemplos

```java
// Libro "1" *-- "1..*" Ejemplar (composición: el Libro crea sus Ejemplares y nace con al menos uno)
public class Libro {
    private List<Ejemplar> ejemplares = new ArrayList<>();

    public Libro(String codigoPrimerEjemplar) {
        ejemplares.add(new Ejemplar(codigoPrimerEjemplar));
    }

    public void agregarEjemplar(String codigo) {
        ejemplares.add(new Ejemplar(codigo));
    }
}

// Alumno "1" --> "0..3" Prestamo (asociación con máximo 3)
public class Alumno {
    private List<Prestamo> prestamos = new ArrayList<>();

    public void agregarPrestamo(Prestamo p) {
        if (prestamos.size() >= 3) {
            throw new IllegalStateException("Máximo 3 préstamos");
        }
        prestamos.add(p);
    }
}
```

## Checklist

- [ ] Atributos `private`; estereotipo como comentario.
- [ ] Composición con `new` dentro de la clase; agregación con el objeto recibido de afuera.
- [ ] Multiplicidad reflejada (objeto vs `List`) y el límite validado donde se agrega.
- [ ] Todos los mensajes de la secuencia existen como métodos.
