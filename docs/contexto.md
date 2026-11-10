# Contexto del sistema: Sistema de certificados en línea

> Nivel 1 de C4. El contexto no es una vista de 4+1: es el marco que encuadra a todas. Diagrama: [contexto.puml](diagramas/contexto.puml)

## 1. Presentación principal

El Sistema de certificados se muestra como una caja negra rodeada de sus actores y de los sistemas externos con los que se comunica. Responde: ¿qué hay afuera y cómo se comunica el sistema con ello?

Diagrama: [diagramas/contexto.puml](diagramas/contexto.puml)

## 2. Catálogo de elementos

| Elemento | Tipo | Rol o responsabilidad | Interacción con el sistema |
|----------|------|-----------------------|----------------------------|
| Estudiante o egresado | Persona | Solicita, paga y descarga sus certificados | Aplicación web (HTTPS) |
| Funcionario de Registro | Persona | Revisa las solicitudes con novedades | Aplicación web (HTTPS) |
| Sistema de certificados | Sistema en alcance | Emite certificados académicos firmados digitalmente | Con todos los demás elementos |
| SIA | Sistema externo | Fuente de notas y paz y salvo, administrada por el Área de Sistemas | Consulta por su API REST (HTTPS) |
| Pasarela de pagos | Sistema externo | Cobra el trámite | HTTPS |
| Proveedor de firma | Sistema externo | Firma el PDF; responde entre 2 y 8 s y no garantiza disponibilidad (RT-03) | HTTPS |
| Servicio de correo | Sistema externo | Envía las notificaciones al estudiante | SMTP |

## 3. Justificación

- CU-01, CU-02 y CU-03 (primarios) definen los dos actores y las integraciones críticas.
- RT-03: el proveedor de firma es externo y no se controla; por eso aparece como sistema aparte.
- RT-04: la integración con el SIA se hace solo por su API REST actual.
- Stakeholder: el Administrador del SIA necesita ver qué se consulta y por qué canal.
- ADR-004: la identidad del usuario proviene del mecanismo de autenticación institucional.

## 4. Comportamiento

Los escenarios (+1) empiezan aquí: [CU-01 solicitar certificado](diagramas/secuencias/cu01-solicitar-certificado.puml) con firma asíncrona, y las interacciones con el SIA y la pasarela de pagos.

## 5. Vistas relacionadas

- [Procesos](vistas/02-procesos.md): qué contenedor habla con cada sistema externo.
- [Física](vistas/04-fisica.md): por qué red sale cada conexión.
