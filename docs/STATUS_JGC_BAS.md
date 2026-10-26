# 📊 Estado del Proyecto: JGC BAS SAC - CRM & Agentic Engine
**Fecha de Actualización:** 25 de Octubre de 2026
**Estado:** Hito v0.1.0 Completado + Mejoras de IA

---

## 🎯 1. Resumen Ejecutivo
El proyecto consiste en un **CRM Multi-tenant con Inteligencia Artificial** diseñado para gestionar la cartera de clientes de JGC Building Automation Supplies SAC. El sistema automatiza el análisis de clientes, la gestión del pipeline de ventas, la generación de cotizaciones (PDF/Excel) y la logística básica, todo potenciado por un agente de IA llamado **Jota**.

**Hito más reciente:** Se completó la ingesta y clasificación automática de **181 empresas reales** y **427 contactos** (extraídos del Excel de Catherine), y se integró el motor de IA para sugerir los próximos pasos comerciales basados en las mejores prácticas de Microsoft Dynamics 365.

---

## 🏗️ 2. Arquitectura y Stack Tecnológico

El sistema está diseñado bajo una arquitectura desacoplada y escalable:
*   **Frontend:** HTML5, CSS3, JavaScript vanilla.
    *   *Estructura:* Panel modular (`admin.html` como Shell + carpeta `modulos/` con Dashboard, Cotizaciones, IA y Logística).
    *   *Hosting:* GitHub Pages (rama `main`).
*   **Backend:** API RESTful en Python con **FastAPI**.
    *   *Hosting:* Ubuntu Desktop 24/7 (IdeaCentre) expuesto mediante **Cloudflare Tunnel** (`api.camisale.work`).
*   **Base de Datos:** PostgreSQL 16 (Local).
    *   *Arquitectura:* Multi-tenant con aislamiento por esquemas (`agentic`, `jgc`, `ayllu`).

---

## 🧠 3. Motor de Inteligencia Artificial ("Jota")

La IA opera bajo una arquitectura de resiliencia (Fallback) y utiliza conocimiento contextual (Ground Truth).

*   **Motor Principal:** **Kilo** (API externa, rápido y preciso).
*   **Motor de Respaldo:** **Qwen 1.8b** (Local, vía Ollama, para operación sin internet).
*   **Ground Truth (Matriz de Conocimiento):** La IA lee las tablas `agentic.knowledge_global` (Mejores prácticas de Microsoft/Salesforce) y `jgc.knowledge_lessons` (Reglas del negocio) antes de responder.
*   **Lógica de Acciones (Next Steps):** 
    *   Jota analiza el historial del cliente, su Score y si los pasos anteriores ya fueron completados.
    *   Genera 2 acciones concretas y el vendedor puede marcarlas con un "Checkmark" (✓) al realizarlas.
    *   Al solicitar nuevas sugerencias, Jota lee el estado anterior y genera los *siguientes pasos lógicos* para avanzar la venta.

---

## 🗄️ 4. Esquema de Base de Datos (Schema `jgc`)

*   **`clientes`:** 181 registros (RUC, Razón Social, Score A/B/C, Segmento, etc.).
*   **`contactos`:** 427 registros (Nombre, Email, Teléfono, Cargo) asociados a múltiples empresas.
*   **`oportunidades`:** Pipeline de ventas (Prospección, Cotización, Negociación, Ganada/Perdida).
*   **`productos`:** 23 productos reales del catálogo (Danfoss, Greystone, Dwyer, APC, Winters).
*   **`chat_historial`:** Memoria de conversaciones de la IA (Temporalmente deshabilitado en UI).
*   **`memoria_agente`:** Registros de los análisis ejecutivos generados por la IA.

---

## ✅ 5. Funcionalidades Completadas (Features)

### A. Dashboard y Gestión de Clientes
*   KPIs visuales (Total Empresas, Distribución por Score).
*   Gráficos interactivos (Chart.js).
*   Buscador dinámico de empresas.
*   Visor de contactos por empresa.

### B. Gestión de Oportunidades y Cotizaciones
*   Creación y edición de oportunidades con código único (`OPP-2026-XXXX`).
*   Avance de embudo (Prospecto -> Cotización -> Negociación -> Ganada).
*   **Generador de Cotizaciones:** Modal completo con cálculo automático de IGV (18%) y exportación nativa a PDF y Excel.

### C. Logística e Inventario
*   Tabla de gestión de 23 productos con actualización de stock y precios base.
*   Alertas visuales cuando el stock es menor o igual a 3 unidades.

---

## ⏳ 6. Próximos Pasos y Roadmap (v0.2.0)

1.  **Autenticación Segura:** Migrar el login simple a **Google Workspace OAuth** (solo usuarios `@jgcbas.com`).
2.  **Automatización de Reportes (Cron):** Script diario a las 8:00 AM para que Jota genere el "Resumen Ejecutivo" antes de que Catherine empiece a trabajar.
3.  **Envío Automático de Cotizaciones:** Integrar el API de Gmail o WhatsApp Business directamente desde el botón de cotizar, sin depender de `mailto:`.
4.  **Chat Conversacional (Beta):** Reactivar el chat de Jota utilizando RAG (Retrieval-Augmented Generation) para evitar alucinaciones del modelo local.
5.  **Alta Disponibilidad:** Implementar un servidor Master-Slave (Keepalived + Postgres Streaming).

---
*Documento generado automáticamente por el equipo de desarrollo de Agentic Engine.*
