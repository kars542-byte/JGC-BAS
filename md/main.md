# Brief Técnico · Proyecto JGC BAS SAC
## Handoff para el motor agéntico (camisale.work)

**Fecha:** Octubre 2026 (última actualización: 10/10/2026)
**Autor:** Kenneth
**Destino:** Conversación del proyecto agentic engine (camisale.work)
**Cliente:** JGC Building Automation Supplies SAC
**Estado:** Cotización aprobada. Diseño pagado (S/ 300). En ejecución.

---

## 1. Contexto del negocio

**Cliente:** JGC Building Automation Supplies SAC
**RUC:** 20607166685
**Giro:** Comercialización B2B y representación de marcas líderes en automatización para edificios, control e instrumentación para sistemas HVAC y procesos industriales.

**Marcas que representa:**
- **Greystone Energy Systems** (representante exclusivo en Perú)
- **Dwyer Omega**
- **Danfoss**
- **Air Products & Controls (APC)**
- **Winters Instruments**

**Dominio propio:** jgcbas.com (registrado en Squarespace Domains desde 04/10/2022)
**Correo corporativo:** Google Workspace Business Starter
- jsaldarriaga@jgcbas.com
- adm@jgcbas.com

**Contactos clave:**
- **Catherine Ruiz** — Administración y Finanzas · adm@jgcbas.com · +51 919 758 580
- **Joel Saldarriaga** — Jefe Técnico Comercial · jsaldarriaga@jgcbas.com · +51 953 299 953
- **Miguel Ruiz** — Asistente de gestión integral · adm@jgcbas.com · +51 961 866 051

**Dirección:** Jr. Pedro Garezon 189, piso 3, San Miguel, Lima – Perú, C.P. 15088
**Google Maps:** https://maps.app.goo.gl/PXDDiL88aPLjkhLx8
**Horario de atención:** Lunes a Viernes · 9:00 A.M. – 5:00 P.M.

**Redes sociales:**
- LinkedIn: https://www.linkedin.com/company/jgc.bas (primera prioridad)
- Facebook: https://www.facebook.com/JGCBASSAC
- Instagram: https://www.instagram.com/jgc.bas
- TikTok: https://www.tiktok.com/@jgcbas

---

## 2. Objetivo del proyecto

Construir un **sistema web comercial con IA de gestión de CRM** para JGC BAS SAC que permita:

1. Centralizar la gestión de **500 clientes** (con subredes/segmentos debajo de cada uno).
2. Automatizar el seguimiento comercial y la generación de cotizaciones.
3. Detectar clientes inactivos y oportunidades de expansión.
4. Generar **sugerencias semanales de expansión** basadas en el comportamiento de la empresa.
5. Servir como base para un futuro **módulo de Post Venta** (seguimiento técnico + tickets de incidencias).

**Visión a mediano plazo:** este sistema es la base del **ERP verticalizado para automatización B2B en Perú**, que se ofrecerá a otros clientes por licencia de uso.

---

## 3. Alcance contratado (4 ítems)

| Ítem | Descripción | Tipo | Inversión | Plazo |
|------|-------------|------|-----------|-------|
| 1. Diseño | Diseño web corporativo + interfaz del panel CRM. Hasta 12 rondas de ajustes sobre diseño predeterminado. | Pago único | S/ 300 | 3 días |
| 2. Desarrollo | Web corporativa + IA de Gestión de CRM. | Pago único | S/ 1,150 | 15 días |
| 3. Implementación | Configuración servidor, despliegue, carga de datos, hasta 2 capacitaciones. | Pago único | S/ 250 | 2 días |
| 4. Mantenimiento | Hosting, monitoreo, respaldos, hasta 3h/mes de ajustes menores. | Mensual | S/ 75/mes | Recurrente |

**Total inicial:** S/ 1,700
**Recurrente:** S/ 75/mes (precio de lanzamiento válido 12 meses)

**Estado de pagos:**
- ✅ Diseño pagado (S/ 300) el 07/10/2026
- ⏳ Desarrollo: 50% al inicio (S/ 575) + 50% contra entrega (S/ 575)
- ⏳ Implementación: 50% + 50%
- ⏳ Mantenimiento: facturación mensual

---

## 4. Cronograma de ejecución

| Fase | Fecha inicio | Fecha fin | Entregable |
|------|--------------|-----------|------------|
| Diseño | 08/10/2026 | 10/10/2026 | Wireframes + diseño web aprobado |
| Desarrollo | 13/10/2026 | 31/10/2026 | Sistema funcional con datos de prueba |
| Implementación | 03/11/2026 | 04/11/2026 | Sistema en producción + 2 capacitaciones |
| Mantenimiento | 05/11/2026 | Continuo | Soporte mensual + respaldos |

**Fecha objetivo de entrega final:** 04/11/2026

---

## 5. Arquitectura técnica objetivo

### 5.1 Principios
- **Sin dependencia de Supabase** para JGC (por costo, control y aprendizaje).
- **Base de datos local** en servidor propio con Ubuntu.
- **Alta disponibilidad** con esquema 1+1 (Master/Slave).
- **Motor agéntico reutilizable** en camisale.work para multi-tenant.

### 5.2 Servidor 1+1 (Master/Slave)

**Hardware:**
- 2 máquinas con Ubuntu (una de ellas es Lenovo IdeaCentre de casa).
- PostgreSQL en ambas.

**Configuración recomendada:**
- **Master:** PostgreSQL con `wal_level = replica`. Usuario `replicator`.
- **Slave:** `pg_basebackup` + `primary_conninfo` para streaming replication.
- **IP Flotante (VIP):** **Keepalived** para failover automático.
- **Pool de conexiones:** **PgBouncer** para no saturar PostgreSQL con 500 clientes.

**Nota:** La sincronización por intervalos de 12h es posible con **WAL Archiving**, pero la recomendación técnica es **streaming replication continua** para failover real sin pérdida de datos.

### 5.3 Relación con camisale.work

**Contexto:**
- Ya existe un **motor agéntico (API) corriendo en camisale.work**.
- El avance actual está construido sobre **Supabase**.
- La meta es **reorientar ese avance** para que cubra JGC como primer tenant productivo.

**Estrategia multi-tenant:**
1. camisale.work como núcleo del motor agéntico (orquestación IA, sugerencias, reportes).
2. Capa de datos abstraída para soportar múltiples backends:
   - Supabase (para clientes que prefieran SaaS).
   - PostgreSQL self-hosted (para JGC y clientes con soberanía de datos).
3. Diseño multi-tenant desde el inicio:
   - `tenant_id` por cliente (JGC = primer tenant).
   - Políticas de aislamiento por tenant.
4. Portar la lógica ya construida en Supabase hacia una capa agnóstica de base de datos.

---

## 6. Requerimientos del CRM

### 6.1 Gestión de clientes
- Base de datos para **500 clientes**.
- **Subredes / segmentos** debajo de cada cliente (relación jerárquica).
- Campos mínimos por cliente:
  - Razón social / nombre
  - RUC
  - Contacto (nombre, teléfono, correo)
  - Historial de compras (producto, fecha, monto)
  - Fecha del último contacto
  - Notas personalizadas
  - Etiquetas / segmentos
  - Score de valor (A / B / C)

### 6.2 Segmentación en grupos de interés

**Criterios:**
1. **Temporalidad:** Frecuente / Estacional / Inactivo (+2 meses) / Inactivo crítico (+6 meses)
2. **Tipo de negocio:** HVAC / Industria / Edificaciones / Tratamiento de agua / Detección y control / Instrumentación
3. **Tipo de hardware:** Variadores Danfoss / Sensores Greystone / Transmisores Dwyer / Detección APC / Manómetros Winters
4. **Valor:** A (alto) / B (medio) / C (bajo)

### 6.3 Generación de cotizaciones
- Generador de PDF con formato JGC (reutiliza identidad visual).
- Precarga de productos comprados anteriormente.
- Registro de estado: pendiente / aprobada / rechazada.

### 6.4 Panel de administración
- CRUD de clientes.
- Vista de historial por cliente.
- Filtros por segmento, temporalidad, marca, valor.
- Reportes comerciales periódicos.

---

## 7. Requerimientos de la IA de Gestión

### 7.1 Funciones principales
- **Trabajar con datos y oportunidades del entorno JGC.**
- **Sugerencias semanales de expansión** basadas en el comportamiento de la empresa.
- Detección automática de clientes inactivos.
- Alertas cuando un cliente no ha sido contactado en X tiempo.
- Recomendaciones de próximos pasos por cliente.

### 7.2 Ejemplos de sugerencias esperadas
- *"El grupo de clientes que compró variadores Danfoss hace 12 meses no ha vuelto a comprar. Sugerimos campaña de mantenimiento preventivo."*
- *"Hay 23 clientes del sector HVAC sin contacto en los últimos 3 meses. Priorizar contacto esta semana."*
- *"El cliente X tiene 3 subredes activas. Sugerimos ofrecerle un contrato marco anual."*

### 7.3 Modelo de IA
- Uso actual: **Kilo y Q en modo gratuito** (costo cero inicial).
- Sistema preparado para **escalar a modelo pago** si el volumen lo exige (sin cambiar arquitectura).

---

## 8. Etapas básicas de gestión de leads
Captación → 2. Calificación → 3. Seguimiento → 4. Cotización → 5. Cierre


- **Captación:** registro inicial del lead (origen, interés).
- **Calificación:** asignación de segmento y score.
- **Seguimiento:** contacto, notas, próxima acción.
- **Cotización:** generación de PDF, envío, registro de estado.
- **Cierre:** ganado / perdido, con motivo.

---

## 9. Módulo Post Venta (fase futura, NO cotizada)

**Alcance sugerido:**
- Gestión de seguimiento de clientes con enfoque altamente técnico.
- Gestión de tickets de incidencias como oportunidad comercial.
- Historial técnico por cliente y por equipo instalado.
- Alertas de mantenimiento preventivo y renovación.

**Cotización estimada:** S/ 2,000 - 3,500 (por separado)

---

## 10. Roadmap

| Fase | Alcance | Estado |
|------|---------|--------|
| Diseño | Wireframes + UI | 🟡 En ejecución |
| Desarrollo | Web + IA CRM | ⏳ Pendiente |
| Implementación | Servidor + capacitación | ⏳ Pendiente |
| Mantenimiento | Soporte continuo | ⏳ Pendiente |
| Post Venta | Tickets + seguimiento | 🔮 Futuro |
| ERP multi-tenant | Licenciamiento | 🔮 Futuro |

---

## 11. Publicación web

**Dominio:** jgcbas.com (Squarespace Domains)

**Registros DNS a configurar:**
- **A (raíz @):**
  - 185.199.108.153
  - 185.199.109.153
  - 185.199.110.153
  - 185.199.111.153
- **CNAME (www):** `usuario.github.io`
- **TXT:** valor de verificación que da GitHub
- **MX:** NO modificar (Google Workspace)

**Importante:** Activar "Enforce HTTPS" en GitHub Pages.

---

## 12. Estrategia comercial (contexto interno)

- **Modelo:** Land & Expand (Huawei-style).
- **Primer cliente:** laboratorio de aprendizaje + portafolio.
- **Cliente 2+:** precios de mercado (S/ 3,000 - 5,000 por setup).
- **Cliente 3+:** SaaS / licencia por uso.
- **Meta:** ERP verticalizado para automatización B2B en Perú.

---

## 13. Preguntas abiertas para el motor agéntico

1. ¿Cuál es el estado actual del API en camisale.work? ¿Qué endpoints existen?
2. ¿Qué parte del flujo CRM está implementada en Supabase?
3. ¿Cómo se maneja actualmente la autenticación y multi-tenancy?
4. ¿Qué capa de abstracción se necesita para migrar de Supabase a PostgreSQL self-hosted?
5. ¿Qué lógica de IA ya existe y cómo se reutiliza para JGC?
6. ¿Qué componentes deben reescribirse para soportar tenants y segmentación avanzada?

---

## 14. Siguiente paso inmediato

1. **Desde camisale.work:** evaluar arquitectura actual y proponer plan de adaptación multi-tenant.
2. **Desde JGC:** completar diseño (en curso) y validar con Catherine funcionalidades del CRM.
3. **Infraestructura:** montar servidor Ubuntu 1+1 (Master/Slave) con PostgreSQL + Keepalived + PgBouncer.
4. **Publicación:** configurar DNS en Squarespace para apuntar a GitHub Pages.

---

**Fin del brief.**
