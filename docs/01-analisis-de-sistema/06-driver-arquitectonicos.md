# Drivers Arquitectónicos

Los drivers arquitectónicos son aquellos factores (requisitos funcionales clave, atributos de calidad críticos y restricciones técnicas) que determinan de manera prioritaria las decisiones sobre la organización, límites y tecnologías de la arquitectura[cite: 1].

| ID | Driver Arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- |
| **DA01** | El sistema debe soportar un incremento sustancial de usuarios concurrentes durante campañas comerciales[cite: 1]. | **AC03 - Escalabilidad**[cite: 1] | Condiciona la estrategia de despliegue, la necesidad de servicios sin estado y la capacidad de escalamiento horizontal de la solución[cite: 1]. |
| **DA02** | El sistema debe mantener tiempos de respuesta bajos (baja latencia) bajo alta concurrencia de consultas[cite: 1]. | **AC01 - Rendimiento**[cite: 1] | Determina los patrones de comunicación entre componentes, optimización de consultas y posibles estrategias de almacenamiento en caché[cite: 1]. |
| **DA03** | El sistema debe proteger los datos personales, cuentas y transacciones financieras[cite: 1]. | **AC04 - Seguridad**[cite: 1] | Obliga a implementar mecanismos formales de autenticación, autorización por roles (Cliente, Seller, Admin) y cifrado en tránsito y reposo[cite: 1]. |
| **DA04** | El sistema debe integrarse con una pasarela de pago externa mediante API[cite: 1]. | **RC04 - Pasarela de pago**[cite: 1] | Condiciona la comunicación segura con terceros, manejo de webhooks/callbacks y desacoplamiento de la lógica de cobro[cite: 1]. |
| **DA05** | El sistema debe utilizar una API REST para la comunicación entre el frontend y el backend[cite: 1]. | **RC03 - API REST**[cite: 1] | Define el contrato y protocolo de desacoplamiento de la capa de presentación respecto a la capa de lógica del negocio[cite: 1]. |
| **DA06** | El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | **AC05 - Mantenibilidad** | Influye en la separación de responsabilidades, modularidad interna y el control estricto de dependencias hacia el dominio. |