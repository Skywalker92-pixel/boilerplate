# Estilo Arquitectónico: Monolito Modular en Capas

## 1. Justificación del Estilo Global
Para el Marketplace de productos para mascotas se ha seleccionado el estilo de **Monolito Modular**, complementado con una organización lógica en capas. 

* **Unidad de Despliegue Única:** Todo el backend se empaqueta y ejecuta como un solo servicio en Node.js, reduciendo costos y complejidad operacional en etapas iniciales.
* **Aislamiento Modular:** Las capacidades del negocio se segregan en módulos bien definidos (`Usuarios`, `Sellers`, `Catálogo`, `Carrito`, `Pedidos`), evitando dependencias circulares y llamadas cruzadas descontroladas.
* **Comunicación Externa:** Se expone una API REST para clientes web/móviles y se orquesta la integración con pasarelas de pago y servicios de envío mediante adaptadores.

## 2. Diagrama del Sistema (Mermaid)

```mermaid
flowchart TD
    subgraph CLIENTES ["Capa de Clientes"]
        WebClient["Aplicación Web (Cliente / Seller / Admin)"]
    end

    subgraph MONOLITO ["Marketplace Backend (Monolito Modular - Node.js / Express)"]
        direction TB
        MW["Middlewares Transversales (CORS, JWT Auth, Validación, Logger)"]
        
        subgraph PRESENTACION ["1. Capa de Presentación (Rutas y Controladores)"]
            direction LR
            U_R["usuarios.routes"]
            S_R["sellers.routes"]
            CAT_R["catalogo.routes"]
            CAR_R["carrito.routes"]
            P_R["pedidos.routes"]
        end

        subgraph LOGICA ["2. Capa de Lógica de Negocio (Servicios y Casos de Uso)"]
            direction LR
            U_S["usuarios.service"]
            S_S["sellers.service"]
            CAT_S["catalogo.service"]
            CAR_S["carrito.service"]
            P_S["pedidos.service"]
        end

        subgraph DATOS ["3. Capa de Acceso a Datos (Repositorios)"]
            direction LR
            U_D["usuarios.repo"]
            S_D["sellers.repo"]
            CAT_D["catalogo.repo"]
            CAR_D["carrito.repo"]
            P_D["pedidos.repo"]
        end
    end

    subgraph PERSISTENCIA ["Almacenamiento & Caché"]
        PostgreSQL[("PostgreSQL")]
        Redis[("Redis Cache")]
    end

    subgraph EXTERNOS ["Sistemas Externos"]
        Pasarela["Pasarela de Pagos"]
        Envios["Servicio de Envíos"]
        ERP["ERP / Facturación"]
    end

    %% Relaciones de flujo
    WebClient -->|HTTPS / REST| MW
    MW --> PRESENTACION
    PRESENTACION --> LOGICA
    LOGICA --> DATOS
    DATOS --> PostgreSQL
    LOGICA -.-> Redis
    P_S -->|HTTPS / REST| Pasarela
    P_S -->|HTTPS / REST| Envios
    CAT_S -.-> ERP