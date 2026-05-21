# Manual de Explotación del Sistema ERP y Facturación Dinámica
**Cliente:** WillmanTech S.L.  
**Normativa de Referencia:** ISO/IEC/IEEE 26514:2022 (Systems and software engineering — Design and development of information for users)

## 1. Introducción y Arquitectura

### 1.1 Módulos del Sistema Activados
    La plataforma ERP core de WillmanTech S.L. se compone de una arquitectura modular integrada que mitiga los silos de información:
        `account` (Contabilidad/Facturación):** Motor central para la gestión de asientos contables, devengos de impuestos y flujos de facturas nacionales e internacionales.
        `sale` (Gestión de Ventas):** Captura de presupuestos y conversión automatizada a órdenes de venta vinculadas al módulo contable.
        `product` (Catálogo Maestro):** Almacén lógico indexado de SKUs, tarifas de precios y tipos de tasas impositivas.

### 1.2 Topología de Despliegue Lógico
    El entorno se distribuye de forma aislada en contenedores de microservicios administrados mediante Docker Compose.

    [ Tráfico HTTPS (Puerto 443) ]
                                |
                        [ Proxy Inverso Nginx ]
                                |
           +--------------------+--------------------+
           | (Puerto 8069)                           | (Puerto 8072)
    [ Servicio ERP Core ]                     [ Live Chat / Cron ]
    (Imagen: odoo:17.0-custom)                (Escucha Longpolling)
           |                                         |
           +--------------------+--------------------+
                                |
                 [ Red Interna Docker Bridge ]
                                |
                   [ Motor de Base de Datos ]
                 (Imagen: postgres:15-alpine)

## 2. Guía de Instalación y Reinstalación

    Siga rigurosamente estos pasos para levantar el entorno productivo de manera determinista.

### 2.1 Requisitos Previos del Sistema
    Docker Engine v24.0.0 o superior.
    Docker Compose v2.20.0 o superior.
    Mínimo 4 GB de RAM disponibles y un procesador de 2 Cores.

### 2.2 Variables de Entorno (`.env`)
    Genere un archivo `.env` en la raíz del proyecto con la siguiente estructura de claves seguras:

    ```env
    POSTGRES_DB=willmantech_prod
    POSTGRES_USER=willman_admin
    POSTGRES_PASSWORD=HazUnicaEstaClave2026!
    ERP_ADMIN_PASSWORD=MasterKeyWillman2026$