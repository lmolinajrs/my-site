---
title: "My First Remote Code Execution"
draft: false
weight: 50
contributors: ["Kryot"]
date: 2021-11-11T09:19:42+01:00
---

### RESUMEN
 Para la explotación del mismo, primero fue necesario explotar una sql injection para obtener el usuario y contraseña de los empleados registrados en la aplicación. Posteriormente, gracias a formulario de PQRS (Peticiones, quejas, reclamos y sugerencias) que permitía adjuntar archivos fue posible ejecutar comandos en el servidor.

 ### SQL INJECTION