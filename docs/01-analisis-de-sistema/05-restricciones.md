# Restricciones del Sistema

Las restricciones representan las condiciones, estándares y limitaciones tecnológicas u organizacionales que deben respetarse obligatoriamente durante el diseño y construcción del marketplace.

| ID | Restricción | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| **RC01** | **Aplicación web** | Plataforma | El sistema debe desarrollarse como una aplicación accesible e interactiva mediante un navegador web. |
| **RC02** | **Control de versiones** | Metodología / Proyecto | El código fuente y la documentación deben gestionarse de manera colaborativa utilizando Git y GitHub. |
| **RC03** | **API REST** | Arquitectura / Comunicación | La comunicación e intercambio de datos entre la capa de presentación (frontend) y los servicios de backend debe realizarse estrictamente mediante una API REST[cite: 1]. |
| **RC04** | **Pasarela de pago externa** | Integración externa | El marketplace debe integrarse con una pasarela de pago externa para el procesamiento de transacciones financieras[cite: 1]. |
| **RC05** | **Servicio de envío externo** | Integración externa | La logística, estimación de tarifas y seguimiento de despachos debe delegarse e integrarse con un servicio externo de transporte/envíos[cite: 1]. |