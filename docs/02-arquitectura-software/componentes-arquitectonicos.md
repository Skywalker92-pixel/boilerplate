# Componentes arquitectónicos del Marketplace

## 1. Objetivo

Este documento describe los principales componentes arquitectónicos del sistema Marketplace de productos para mascotas.

El sistema se organiza como un **monolito modular**, donde las funcionalidades del negocio se encuentran separadas en módulos con responsabilidades específicas.

Aunque los módulos forman parte de una misma aplicación y se despliegan juntos, cada uno debe mantener una responsabilidad definida y comunicarse con los demás mediante interfaces públicas, evitando acceder directamente a las tablas o detalles internos de otros módulos.

---

## 2. Arquitectura general

El Marketplace está compuesto por los siguientes elementos principales:

| Elemento | Tipo | Responsabilidad |
|---|---|---|
| Cliente | Actor | Compra productos disponibles en el Marketplace. |
| Seller | Actor | Publica y gestiona sus productos. |
| Administrador | Actor | Gestiona usuarios, productos y pedidos. |
| Aplicación Web | Contenedor | Proporciona la interfaz para clientes, sellers y administradores. |
| API Marketplace | Contenedor | Implementa la lógica del sistema mediante Node.js y Express utilizando un monolito modular. |
| Base de datos | Contenedor | Almacena usuarios, productos, pedidos y demás información persistente. |
| Caché | Contenedor | Mantiene información temporal del catálogo para mejorar el rendimiento. |
| Pasarela de pago | Sistema externo | Procesa los pagos realizados por los clientes. |
| ERP | Sistema externo | Permite la integración con la gestión interna de la empresa. |
| Servicio de envío | Sistema externo | Gestiona la entrega de los pedidos. |

---

## 3. Diagrama de componentes

El siguiente diagrama representa los principales componentes internos de la API del Marketplace y sus relaciones con los usuarios, la aplicación web, los almacenes de datos y los sistemas externos.

![Diagrama de componentes del Marketplace](../img/diagrama-componentes-marketplace.png)

---

## 4. API REST

La **API REST** funciona como el punto de entrada hacia el backend del Marketplace.

### Responsabilidades

- Exponer los endpoints de la API bajo rutas como `/api/v1`.
- Recibir solicitudes de la aplicación web.
- Validar los datos enviados por los usuarios.
- Gestionar autenticación mediante JWT.
- Aplicar middlewares antes de ejecutar las operaciones.
- Invocar los casos de uso correspondientes de los módulos del negocio.
- Devolver respuestas mediante HTTP/JSON.

La API REST no debe contener directamente las reglas principales del negocio. Su responsabilidad es recibir las solicitudes y dirigirlas hacia el módulo correspondiente.

---

## 5. Módulos del negocio

### 5.1. Módulo Usuarios

**Responsabilidad principal:** gestionar las cuentas y la autenticación de los usuarios.

Funciones principales:

- Registro de usuarios.
- Inicio de sesión.
- Autenticación.
- Gestión de roles y permisos.
- Validación de cuentas.

Este módulo proporciona servicios relacionados con la identidad de los usuarios que pueden ser utilizados por otros módulos del sistema.

---

### 5.2. Módulo Sellers

**Responsabilidad principal:** gestionar a los vendedores registrados en el Marketplace.

Funciones principales:

- Registrar sellers.
- Gestionar información de vendedores.
- Validar la cuenta asociada al seller.
- Gestionar información relacionada con sus productos.

El módulo Sellers utiliza los servicios públicos del módulo Usuarios para validar la información correspondiente a las cuentas.

---

### 5.3. Módulo Catálogo

**Responsabilidad principal:** gestionar los productos que se ofrecen dentro del Marketplace.

Funciones principales:

- Registrar productos.
- Consultar productos.
- Actualizar productos.
- Gestionar categorías.
- Gestionar información de stock.
- Proporcionar información de precio y disponibilidad.

El módulo Catálogo puede utilizar una caché para reducir las consultas repetitivas a la base de datos.

---

### 5.4. Módulo Carrito

**Responsabilidad principal:** administrar temporalmente los productos que un cliente desea comprar.

Funciones principales:

- Agregar productos al carrito.
- Modificar cantidades.
- Eliminar productos.
- Consultar precios.
- Consultar disponibilidad y stock.
- Calcular la información necesaria para iniciar una compra.

El módulo Carrito consulta información del módulo Catálogo mediante su interfaz pública.

---

### 5.5. Módulo Pedidos

**Responsabilidad principal:** gestionar el proceso de compra y los estados de los pedidos.

Funciones principales:

- Crear un pedido a partir del carrito.
- Ejecutar el proceso de checkout.
- Gestionar los estados del pedido.
- Solicitar el procesamiento del pago.
- Solicitar el despacho del pedido.
- Sincronizar información del pedido con el ERP.
- Generar eventos o cambios de estado para las notificaciones.

El módulo Pedidos actúa como uno de los módulos centrales del proceso de compra.

---

### 5.6. Módulo Pagos

**Responsabilidad principal:** gestionar el cobro correspondiente a los pedidos.

Funciones principales:

- Solicitar un cobro.
- Comunicarse con la pasarela de pago.
- Procesar la respuesta del proveedor externo.
- Informar el resultado del pago al proceso de pedido.

La comunicación con la pasarela de pago se realiza mediante una integración externa utilizando HTTPS/REST.

---

### 5.7. Módulo Envíos

**Responsabilidad principal:** gestionar la información relacionada con el despacho y entrega de los pedidos.

Funciones principales:

- Solicitar un despacho.
- Crear una solicitud de envío.
- Consultar información del envío.
- Comunicarse con el servicio externo encargado de las entregas.

La integración con el servicio de envío se realiza mediante HTTPS/REST.

---

### 5.8. Módulo Notificaciones

**Responsabilidad principal:** informar al cliente sobre acontecimientos importantes relacionados con su pedido.

Funciones principales:

- Recibir cambios en el estado del pedido.
- Generar notificaciones.
- Informar al cliente sobre el avance de su compra.

Este módulo evita que la lógica de notificación quede directamente acoplada al módulo Pedidos.

---

## 6. Contenedores de datos

### 6.1. Base de datos PostgreSQL

El Marketplace utiliza una base de datos relacional PostgreSQL para almacenar la información persistente del sistema.

Entre los principales datos almacenados se encuentran:

- Usuarios.
- Sellers.
- Productos.
- Categorías.
- Carritos.
- Pedidos.
- Pagos.
- Información de envíos.

Cada módulo debe trabajar con las tablas correspondientes a su responsabilidad.

Un módulo no debe acceder directamente a las tablas pertenecientes a otro módulo.

---

### 6.2. Caché Redis

Redis se utiliza como mecanismo de caché para información que puede ser consultada frecuentemente.

En el Marketplace se utiliza principalmente para almacenar temporalmente información relacionada con el catálogo.

Su objetivo es reducir consultas repetitivas y mejorar el rendimiento de las operaciones de lectura.

---

## 7. Sistemas externos

### Pasarela de pago

Sistema externo encargado de procesar los pagos realizados durante una compra.

La comunicación se realiza desde el módulo Pagos mediante HTTPS/REST.

---

### ERP

Sistema externo utilizado para integrar los pedidos del Marketplace con los procesos internos de la empresa.

La responsabilidad de integración se asigna inicialmente al módulo Pedidos.

> **Nota:** esta asignación debe ser confirmada posteriormente con el negocio.

---

### Servicio de envío

Sistema externo encargado de gestionar las entregas.

El módulo Envíos se comunica con este servicio para crear y consultar la información asociada al despacho de los pedidos.

---

## 8. Relaciones entre componentes

| Componente origen | Relación | Componente destino |
|---|---|---|
| Aplicación Web | Invoca mediante HTTPS/JSON | API REST |
| API REST | Invoca casos de uso | Módulos del negocio |
| Sellers | Valida cuenta | Usuarios |
| Sellers | Asocia productos | Catálogo |
| Carrito | Consulta precio y stock | Catálogo |
| Carrito | Se convierte en pedido | Pedidos |
| Pedidos | Solicita cobro | Pagos |
| Pedidos | Solicita despacho | Envíos |
| Pedidos | Publica cambio de estado | Notificaciones |
| Catálogo | Almacena información temporal | Caché Redis |
| Pagos | Procesa cobro mediante HTTPS/REST | Pasarela de pago |
| Pedidos | Sincroniza pedidos mediante HTTPS/REST | ERP |
| Envíos | Crea envío mediante HTTPS/REST | Servicio de envío |
| Módulos del negocio | Leen y escriben sus datos | PostgreSQL |

---

## 9. Reglas de comunicación entre módulos

La arquitectura modular establece las siguientes reglas:

1. Cada componente de negocio representa un módulo del monolito.
2. Todos los módulos forman parte de una única aplicación y se despliegan juntos.
3. Cada módulo debe mantener una responsabilidad claramente definida.
4. Un módulo debe acceder a otro únicamente mediante su interfaz pública o servicio.
5. Un módulo no debe acceder directamente a las tablas pertenecientes a otro módulo.
6. Las integraciones externas deben mantenerse separadas de las reglas principales del negocio.
7. Las dependencias entre módulos deben mantenerse controladas para favorecer la mantenibilidad del sistema.

---

## 10. Flujo principal de compra

El flujo principal del Marketplace puede representarse de la siguiente manera:

```text
Cliente
   │
   ▼
Aplicación Web
   │
   │ HTTPS / JSON
   ▼
API REST
   │
   ▼
Catálogo
   │
   ▼
Carrito
   │
   ▼
Pedidos
   │
   ├──────────────► Pagos ─────────► Pasarela de pago
   │
   ├──────────────► Envíos ────────► Servicio de envío
   │
   ├──────────────► Notificaciones
   │
   └──────────────► ERP
```

El cliente consulta los productos disponibles, agrega productos al carrito y posteriormente genera un pedido.

A partir del pedido se coordinan las operaciones necesarias para procesar el pago, gestionar el envío, sincronizar información empresarial y comunicar al cliente los cambios de estado.

---

## 11. Relación con los atributos de calidad

La división del sistema en componentes responde también a los atributos de calidad definidos previamente.

| Atributo de calidad | Decisión relacionada |
|---|---|
| Rendimiento | Utilización de Redis como caché para consultas frecuentes del catálogo. |
| Disponibilidad | Separación de las integraciones externas para controlar fallos de servicios externos. |
| Escalabilidad | Organización modular que permite identificar las partes con mayor carga. |
| Seguridad | Autenticación y autorización centralizadas desde la API y el módulo Usuarios. |
| Mantenibilidad | Separación de responsabilidades mediante módulos independientes dentro del monolito. |

---

## 12. Conclusión

La arquitectura del Marketplace se organiza mediante un **monolito modular**, donde cada componente representa una capacidad específica del negocio.

Los módulos Usuarios, Sellers, Catálogo, Carrito, Pedidos, Pagos, Envíos y Notificaciones permiten separar responsabilidades y mantener controladas las dependencias internas.

La API REST funciona como punto de entrada al backend, mientras que PostgreSQL proporciona persistencia, Redis permite implementar caché y los sistemas externos permiten integrar pagos, gestión empresarial y servicios de entrega.

Esta organización establece una base clara para desarrollar posteriormente el diseño interno de los módulos aplicando principios de diseño, patrones y Clean Architecture.