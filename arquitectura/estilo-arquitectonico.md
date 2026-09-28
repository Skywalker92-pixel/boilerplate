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
        subgraph MIDDLEWARES ["Middlewares Universales"]
            CORS["CORS"]
            Auth["Auth (JWT / Roles)"]
            Validacion["Validación de Entrada"]
            Logger["Logs & Métricas"]
        end

        subgraph PRESENTACION ["1. Capa de Presentación (Rutas y Controladores)"]
            U_R["usuarios.routes / controller"]
            S_R["sellers.routes / controller"]
            CAT_R["catalogo.routes / controller"]
            CAR_R["carrito.routes / controller"]
            P_R["pedidos.routes / controller"]
        end

        subgraph LOGICA ["2. Capa de Lógica de Negocio (Servicios y Casos de Uso)"]
            U_S["usuarios.service"]
            S_S["sellers.service"]
            CAT_S["catalogo.service"]
            CAR_S["carrito.service"]
            P_S["pedidos.service"]
        end

        subgraph DATOS ["3. Capa de Acceso a Datos (Repositorios)"]
            U_D["usuarios.repository"]
            S_D["sellers.repository"]
            CAT_D["catalogo.repository"]
            CAR_D["carrito.repository"]
            P_D["pedidos.repository"]
            ORM["Acceso a Datos / Pool Conexiones"]
        end
    end

    subgraph PERSISTENCIA ["Almacenamiento"]
        PostgreSQL[("Base de Datos PostgreSQL")]
        Redis[("Caché Redis")]
    end

    subgraph EXTERNOS ["Sistemas Externos"]
        Pasarela["Pasarela de Pagos (Stripe / Local)"]
        Envios["Servicio de Envíos / Courier"]
        ERP["ERP / Facturación"]
    end

    %% Relaciones
    WebClient -->|HTTPS / REST| MIDDLEWARES
    MIDDLEWARES --> PRESENTACION
    PRESENTACION --> LOGICA
    LOGICA --> DATOS
    DATOS --> ORM
    ORM --> PostgreSQL
    LOGICA -.-> Redis
    P_S -->|HTTPS / REST| Pasarela
    P_S -->|HTTPS / REST| Envios
    CAT_S -.-> ERP