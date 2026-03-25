# 🍽️ BistroStreet — Guia d'Execució Local amb Docker i Node.js

## 📖 Índex

1. [Què és BistroStreet?](#1-què-és-bistrostreet)
2. [Arquitectura del Sistema](#2-arquitectura-del-sistema)
3. [Requisits Previs](#3-requisits-previs)
4. [Instal·lació Pas a Pas](#4-installació-pas-a-pas)
5. [Accés a les Interfícies](#5-accés-a-les-interfícies)
6. [Comandes Útils](#6-comandes-útils)
7. [Estructura del Projecte](#7-estructura-del-projecte)
8. [Resolució de Problemes](#8-resolució-de-problemes)

---

## 1. 📋 Què és BistroStreet?

BistroStreet és un **sistema web dual per a restaurants** que combina dues interfícies en una sola aplicació:

| Interfície | Descripció | Usuaris |
|---|---|---|
| **Client (E-commerce)** | Menú categoritzat, cistella de compra, checkout amb modes Local/Recollida/Delivery | Clients del restaurant |
| **TPV (Punt de Venda)** | Dashboard operatiu, gestió de comandes en temps real, mapa de taules, arqueig de caixa | Cambrers i administradors |

### ✨ Funcionalitats principals

- 🛒 **Comandes en línia** — Menú amb categories, cistella flotant i checkout complet
- 📊 **Dashboard TPV** — Gestió operativa amb mapa de taules interactiu
- ⚡ **Temps real** — Sincronització instantània via WebSocket entre client i TPV
- 🖨️ **Impressió tèrmica** — Integració amb impressora POS-58mm
- 🧾 **Facturació automàtica** — Factures amb numeració correlativa i desglossament d'IVA
- 📦 **Control d'estoc** — Escandallos que descompten ingredients automàticament
- 🎁 **Programa de fidelitat** — Sistema de punts i recompenses per a clients

---

## 2. 🏗️ Arquitectura del Sistema

L'aplicació utilitza una arquitectura de **3 serveis + base de dades**, tot gestionat localment:

```mermaid
graph TB
    subgraph Docker ["🐳 Docker Compose"]
        DB[(PostgreSQL 15<br/>Port 5432)]
        PGA[pgAdmin 4<br/>Port 5050]
    end

    subgraph NodeJS ["⬢ Node.js"]
        BE[Backend API<br/>Express + TypeScript<br/>Port 3000]
        FE[Frontend<br/>Next.js 14 + React<br/>Port 3001]
        PS[Print Server<br/>Express<br/>Port 3002]
    end

    FE -->|REST API| BE
    FE <-->|WebSocket<br/>Socket.io| BE
    BE -->|Sequelize ORM| DB
    BE -->|HTTP| PS
    PGA -->|SQL| DB

    style Docker fill:#2496ED,color:#fff
    style NodeJS fill:#339933,color:#fff
    style DB fill:#336791,color:#fff
    style BE fill:#000000,color:#fff
    style FE fill:#000000,color:#fff
    style PS fill:#000000,color:#fff
