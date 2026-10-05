# Escenarios de atributos de calidad del Marketplace

Los atributos de calidad definidos para el Marketplace deben expresarse mediante escenarios concretos y medibles.

El propósito de estos escenarios es establecer condiciones que permitan verificar posteriormente si la arquitectura propuesta responde adecuadamente a los requisitos de calidad del sistema.

Cada escenario considera una fuente del estímulo, el estímulo que ocurre, las condiciones en las que se produce, la respuesta esperada del sistema y una medida que permita comprobar su cumplimiento.

---

## EQ-01. Rendimiento: consulta de productos

| Elemento | Descripción |
|---|---|
| **Atributo de calidad** | Rendimiento |
| **Fuente** | Cliente |
| **Estímulo** | El cliente busca productos o consulta el detalle de un producto. |
| **Condición** | El Marketplace funciona normalmente y se utilizan datos representativos. |
| **Respuesta esperada** | El sistema muestra los resultados de búsqueda o los detalles del producto sin errores inesperados. |
| **Medida** | **Propuesta por validar:** el 95 % de las consultas responde en 2 segundos o menos, con menos del 1 % de errores técnicos, durante una prueba con 200 usuarios simultáneos. |
| **Verificación** | Ejecutar pruebas de rendimiento y registrar los tiempos de respuesta, los errores y la cantidad de usuarios simultáneos. |
| **Estado** | Pendiente de aprobación de las medidas y definición del entorno de prueba. |

---

## EQ-02. Disponibilidad: continuidad del servicio

| Elemento | Descripción |
|---|---|
| **Atributo de calidad** | Disponibilidad |
| **Fuente** | Cliente |
| **Estímulo** | El cliente intenta realizar una operación durante una campaña comercial o cuando ocurre una falla en un componente. |
| **Condición** | El sistema está publicado y cuenta con monitoreo. |
| **Respuesta esperada** | El sistema mantiene disponibles las operaciones que puede atender y comunica los errores de forma controlada cuando un servicio necesario no responde. |
| **Medida** | **Propuesta por validar:** disponibilidad mensual de 99,5 % para las operaciones críticas. |
| **Verificación** | Revisar los registros de monitoreo y calcular los periodos de disponibilidad e interrupción. |
| **Estado** | Pendiente de aprobar la meta, definir las operaciones críticas y establecer las condiciones de mantenimiento. |

---

## EQ-03. Escalabilidad: aumento de usuarios

| Elemento | Descripción |
|---|---|
| **Atributo de calidad** | Escalabilidad |
| **Fuente** | Incremento de solicitudes de los clientes |
| **Estímulo** | Aumenta la cantidad de usuarios que utilizan el Marketplace simultáneamente. |
| **Condición** | Se realiza una prueba con datos, configuración y solicitudes documentados. |
| **Respuesta esperada** | El sistema soporta el aumento de usuarios sin superar los límites acordados de tiempo de respuesta y errores. |
| **Medida** | **Propuesta por validar:** soportar hasta 200 usuarios simultáneos, con el 95 % de las respuestas por debajo de 3 segundos y menos del 1 % de errores técnicos. |
| **Verificación** | Aumentar progresivamente la carga y comparar los tiempos de respuesta, los errores y el uso de recursos. |
| **Estado** | Pendiente de confirmar la cantidad esperada de usuarios y la infraestructura de prueba. |

---

## EQ-04. Seguridad: acceso a operaciones protegidas

| Elemento | Descripción |
|---|---|
| **Atributo de calidad** | Seguridad |
| **Fuente** | Usuario sin sesión, con credenciales inválidas o sin permisos suficientes |
| **Estímulo** | El usuario intenta acceder a una función protegida o realizar una operación no autorizada. |
| **Condición** | Se realizan pruebas sobre las funciones protegidas del Marketplace. |
| **Respuesta esperada** | El sistema rechaza la operación, no ejecuta el cambio solicitado y no expone información sensible. |
| **Medida** | Rechazar el 100 % de los intentos no autorizados incluidos en las pruebas definidas, sin exposición de datos sensibles en las respuestas revisadas. |
| **Verificación** | Ejecutar pruebas de acceso autorizado y no autorizado, revisar las respuestas y comprobar que no se expongan datos sensibles. |
| **Estado** | Pendiente de definir los permisos por tipo de usuario y las operaciones protegidas. |

---

## EQ-05. Mantenibilidad: cambios sin afectar otras funciones

| Elemento | Descripción |
|---|---|
| **Atributo de calidad** | Mantenibilidad |
| **Fuente** | Equipo de desarrollo |
| **Estímulo** | Se modifica una regla de negocio o se necesita sustituir el proveedor de pagos. |
| **Condición** | El sistema está organizado en módulos y dispone de pruebas para las funciones afectadas. |
| **Respuesta esperada** | El cambio se concentra en el módulo responsable sin afectar funciones no relacionadas, salvo que exista una dependencia justificada. |
| **Medida** | Las pruebas del módulo modificado y las pruebas de regresión acordadas deben aprobarse. Las dependencias entre módulos deben respetar las reglas de arquitectura establecidas. |
| **Verificación** | Revisar los cambios de código, ejecutar las pruebas e inspeccionar las dependencias entre módulos. |
| **Estado** | Pendiente de definir las reglas de dependencias y las pruebas obligatorias para los cambios. |

---

# Resumen de escenarios

| Código | Atributo de calidad | Qué se busca comprobar |
|---|---|---|
| EQ-01 | Rendimiento | Que el Marketplace responda dentro del tiempo establecido. |
| EQ-02 | Disponibilidad | Que las operaciones críticas estén disponibles cuando se necesiten. |
| EQ-03 | Escalabilidad | Que el sistema soporte un aumento de usuarios dentro de los límites definidos. |
| EQ-04 | Seguridad | Que las operaciones protegidas no puedan ejecutarse sin autorización. |
| EQ-05 | Mantenibilidad | Que los cambios puedan realizarse sin afectar innecesariamente otras funciones. |