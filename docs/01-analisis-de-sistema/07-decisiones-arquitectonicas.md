# Registros de Decisiones Arquitectónicas (ADR)

## ADR-001: Adopción del Estilo Monolito Modular
* **Estado:** Aceptado
* **Drivers Relacionados:** DA01 (Escalabilidad), DA06 (Mantenibilidad)
* **Contexto:** Se requiere desplegar el backend de forma sencilla pero conservando aislamiento lógico entre dominios.
* **Decisión:** Implementar un Monolito Modular en Node.js/Express con separación en capas.
* **Consecuencias:** Despliegue unificado, mantenimiento simplificado y bajo costo de infraestructura inicial.

---

## ADR-002: Implementación de Clean Architecture
* **Estado:** Aceptado
* **Drivers Relacionados:** DA06 (Mantenibilidad / Evolución Modular)
* **Contexto:** Las reglas de negocio no deben depender de frameworks ni de librerías de persistencia.
* **Decisión:** Organizar el interior de los módulos en cuatro capas concéntricas con regla de dependencias hacia el dominio.
* **Consecuencias:** Reglas de negocio desacopladas y alta facilidad para pruebas unitarias.

---

## ADR-003: Estrategia de Caché en Memoria
* **Estado:** Aceptado
* **Drivers Relacionados:** DA02 (Rendimiento)
* **Contexto:** Picos de alta concurrencia durante campañas comerciales pueden saturar las consultas de lectura a la base de datos.
* **Decisión:** Implementar Redis como caché para consultas frecuentes de catálogo y categorías.
* **Consecuencias:** Reducción sustancial de latencia y menor carga en PostgreSQL.

---

## ADR-004: Integración Externa mediante Puertos y Adaptadores
* **Estado:** Aceptado
* **Drivers Relacionados:** DA04 (Integración con Pagos y Envíos)
* **Contexto:** Se requiere interactuar con pasarelas de pago y couriers sin acoplar la lógica de compra a sus APIs.
* **Decisión:** Definir interfaces abstractas en el dominio y adaptadores concretos en infraestructura.
* **Consecuencias:** Posibilidad de cambiar de proveedor de pago sin modificar los casos de uso.