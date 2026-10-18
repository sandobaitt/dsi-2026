---
name: dsi-patrones-diseno
description: >-
  Guía de la cátedra para identificar y diagramar Patrones de Diseño (GoF) en Diseño de Sistemas (DSI 2026):
  Singleton, Factory Method, Abstract Factory, Composite, Facade, Proxy, State, Observer e Iterator. Indica cómo elegir el
  patrón según el problema del enunciado, qué rol cumple cada clase del escenario y cómo justificar la elección y
  dibujar el diagrama de clases de la solución.
---

# Skill: Patrones de Diseño - Pautas de Cátedra (DSI 2026)

- **Fuente de verdad:** [apunte.md](../../../apunte.md) → sección *Patrones de diseño* (diagramas PlantUML de cada patrón y "cómo leerlo").
- **Cómo usar los diagramas:** los nombres (*Creador*, *Sujeto*, *Contexto*…) son **roles genéricos**. En el parcial se **reemplazan por las clases del escenario**, indicando qué rol cumple cada una.

## Cómo elegir el patrón (pista del enunciado → patrón)

| Pista en el enunciado | Patrón | Familia |
| :--- | :--- | :--- |
| "una única instancia", "debe ser la misma para todos" (configuración) | **Singleton** | Creación |
| "qué clase concreta instanciar la deciden las subclases", un solo producto | **Factory Method** | Creación |
| "familias de objetos que van juntos", sin conocer clases concretas | **Abstract Factory** | Creación |
| jerarquía parte-conjunto, tratar igual a un elemento y a un grupo (carpeta/archivo) | **Composite** | Estructural |
| interfaz simple sobre varios subsistemas ("confirmarCompra" usa Stock, Pago y Envío) | **Facade** | Estructural |
| controlar el acceso a otro objeto (permisos, carga diferida, remoto) | **Proxy** | Estructural |
| el comportamiento cambia según el estado, sin if/else | **State** | Comportamiento |
| "avisar a varios cuando algo cambia", agregar avisos nuevos **sin modificar** el emisor | **Observer** | Comportamiento |
| recorrer una colección sin exponer su estructura | **Iterator** | Comportamiento |

## Qué entregar (típico: "identificar el patrón, justificar y hacer el diagrama de clases")

1. **Patrón y familia.**
2. **Justificación** atada al **requisito concreto** del enunciado (ej: *"agregar nuevos avisos sin modificar la devolución"* → el Sujeto solo conoce la interfaz Observador; un aviso nuevo es una clase nueva que implementa Observador y se suscribe, sin tocar la lógica de la devolución).
3. **Tabla de roles:** clase del escenario ↔ rol del patrón.
4. **Diagrama de clases** de la solución con interfaces (`<<interface>>` / cursiva), realización (`..|>`), multiplicidades y visibilidad.

## Roles por patrón (resumen)

- **Singleton:** constructor privado `-`, atributo estático `instancia`, `+ getInstancia()`.
- **Factory Method:** `Creador` (abstracto, `crearProducto()` abstracto), `CreadorConcreto` (decide), `Producto` (interfaz), `ProductoConcreto`.
- **Abstract Factory:** `Fabrica` (interfaz con `crearX()` por cada producto), `FabricaConcreta`, interfaces de producto, productos concretos de una misma familia.
- **Composite:** `Componente` (interfaz), `Hoja`, `Compuesto` (`0..*` hijos, `agregar()`/`quitar()`).
- **Facade:** `Cliente → Fachada → Subsistemas`; el cliente no conoce las clases internas.
- **Proxy:** `Sujeto` (interfaz), `Proxy` (guarda `sujetoReal` y decide si delega), `SujetoReal`.
- **State:** `Contexto` (atributo `estado: Estado`, `cambiarEstado()`), `Estado` (interfaz `manejar()`), `EstadoConcreto` por cada estado de la máquina de estados.
- **Observer:** `Sujeto` (lista de `0..*` Observadores; `agregar()`, `quitar()`, `notificar()`), `Observador` (interfaz `actualizar()`), `ObservadorConcreto`.
- **Iterator:** `Agregado` (`crearIterador()`), `Iterador` (`primero()`, `siguiente()`, `hayMas()`, `actual()`), concretos.

## Checklist

- [ ] El patrón elegido responde al requisito de extensibilidad/cambio que pide el enunciado (no solo "se parece").
- [ ] Roles genéricos reemplazados por clases del escenario.
- [ ] Interfaces marcadas y flechas de realización punteadas con triángulo hueco.
- [ ] Multiplicidades en las asociaciones (ej: `Sujeto o-- "0..*" Observador`).
- [ ] Si el patrón es State: coherente con la máquina de estados (un `EstadoConcreto` por estado).
