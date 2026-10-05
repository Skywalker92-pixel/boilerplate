# Enfoque Arquitectónico: Clean Architecture (Arquitectura Limpia)

## 1. Definición y Propósito
Para la organización interna de los módulos del Marketplace de mascotas se adopta **Clean Architecture** (Arquitectura Limpia). 

El objetivo primordial es aislar las reglas y la lógica del negocio del impacto de tecnologías externas (frameworks como Angular o Express, librerías de persistencia, pasarelas de pago y servicios de mensajería).

## 2. El Problema que Resuelve
* **Acoplamiento tecnológico:** Evita que cambios en la API de Stripe, actualización de versiones de Express o migraciones de base de datos rompan las reglas de validación de pedidos o carritos.
* **Dependencias hacia el núcleo:** Aplica el principio de inversión de dependencias: los detalles de infraestructura dependen del dominio, y el dominio no conoce nada del exterior.
* **Testabilidad aislada:** Permite ejecutar pruebas unitarias sobre entidades y casos de uso sin necesidad de levantar bases de datos ni servicios HTTP.

## 3. Organización en Cuatro Capas Concéntricas

| Capa | Responsabilidad | Componentes en el Marketplace |
| :--- | :--- | :--- |
| **1. Dominio (Domain)** | Entidades del negocio y reglas independientes de cualquier tecnología. | `Producto`, `Pedido`, `Cliente`, `Carrito`, reglas de cálculo de total y stock. |
| **2. Aplicación (Casos de Uso)** | Orquesta los flujos de negocio específicos del sistema. | `CrearPedidoUseCase`, `ConfirmarCompraUseCase`, `AgregarAlCarritoUseCase`. |
| **3. Adaptadores (Interface Adapters)** | Traduce datos entre el formato externo y los modelos de casos de uso. | Controladores REST (`PedidoController`), Gateways de pago, Repositorios e interfaces DTO. |
| **4. Infraestructura (Frameworks & Drivers)** | Herramientas concretas, frameworks y librerías externas. | Node.js, Express, PostgreSQL, Stripe SDK, Angular 18. |

## 4. Diagrama de Enfoque Arquitectónico (Mermaid)

```mermaid
flowchart TD
    subgraph INFRAESTRUCTURA ["Capa 4: Infraestructura y Frameworks (Externa)"]
        direction TB
        ExpressFW["Express / HTTP Server"]
        PostgresDB["PostgreSQL / pg Pool"]
        StripeExt["Stripe API / SDK"]
        CourierExt["Servicio de Envíos Courier"]
    end

    subgraph ADAPTADORES ["Capa 3: Adaptadores de Interfaz (Adapters)"]
        direction TB
        Ctrl["PedidoController (Rutas REST)"]
        RepoImpl["PedidoRepositoryPostgreSQL"]
        PayAdapter["StripePaymentAdapter"]
        ShipAdapter["CourierShippingAdapter"]
    end

    subgraph APLICACION ["Capa 2: Casos de Uso (Application Layer)"]
        direction TB
        UC_Crear["CrearPedidoUseCase()"]
        UC_Confirmar["ConfirmarCompraUseCase()"]
        UC_Carrito["GestionarCarritoUseCase()"]
        Ports[("Puertos / Interfaces (Contratos)")]
    end

    subgraph DOMINIO ["Capa 1: Dominio (Domain - Núcleo)"]
        direction TB
        Ent_Pedido["Entidad: Pedido"]
        Ent_Prod["Entidad: Producto"]
        Reglas["Reglas de Negocio (Stock, Validaciones)"]
    end

    %% Relaciones de dependencia hacia adentro
    INFRAESTRUCTURA --> ADAPTADORES
    ADAPTADORES --> APLICACION
    APLICACION --> DOMINIO

    %% Implementación de interfaces (Inversión de dependencias)
    RepoImpl -.->|Implementa| Ports
    PayAdapter -.->|Implementa| Ports
    ShipAdapter -.->|Implementa| Ports