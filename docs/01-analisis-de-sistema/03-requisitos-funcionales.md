# Requisitos Funcionales del Sistema

Los requisitos funcionales determinan las acciones, procesos y servicios específicos que el Marketplace debe proveer para satisfacer las necesidades de los actores.

## 1. Catálogo de Requisitos Funcionales

| ID | Requisito Funcional | Descripción operativa |
| :--- | :--- | :--- |
| **RF01** | Búsqueda de productos | El sistema debe permitir buscar productos mediante criterios de búsqueda (palabras clave, categorías, filtros). |
| **RF02** | Consulta de disponibilidad | El sistema debe permitir consultar la información detallada, precio y disponibilidad de stock de los productos[cite: 1]. |
| **RF03** | Gestión de productos del seller | El sistema debe permitir a los sellers registrar, actualizar y dar de baja productos en la plataforma[cite: 1]. |
| **RF04** | Gestión del carrito de compra | El sistema debe permitir agregar, modificar cantidades y eliminar productos dentro del carrito de compra[cite: 1]. |
| **RF05** | Generación de pedidos | El sistema debe permitir generar un pedido formal a partir de los productos consolidados en el carrito[cite: 1]. |
| **RF06** | Consulta y seguimiento de pedidos | El sistema debe permitir consultar el listado de pedidos realizados y su estado actual en tiempo real[cite: 1]. |
| **RF07** | Gestión de cuentas de sellers | El sistema debe permitir al administrador registrar, validar, actualizar y desactivar cuentas de sellers[cite: 1]. |
| **RF08** | Consulta de detalle de pedido | El sistema debe permitir visualizar el desglose pormenorizado de un pedido realizado (artículos, costo de envío, total e impuestos)[cite: 1]. |

---

## 2. Matriz de Trazabilidad (Historias de Usuario vs. Requisitos Funcionales)

| Historia de Usuario (HU) | Requisitos Funcionales Relacionados |
| :--- | :--- |
| **HU01**: Buscar y consultar productos | RF01, RF02[cite: 1] |
| **HU02**: Gestionar productos | RF03[cite: 1] |
| **HU03**: Gestionar carrito | RF04[cite: 1] |
| **HU04**: Realizar pedido | RF05, RF08[cite: 1] |
| **HU05**: Gestionar sellers | RF07[cite: 1] |
| **HU06**: Consultar pedidos | RF06, RF08[cite: 1] |