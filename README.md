# Automatizaciones N8N - Krak RE

Hub de automatizaciones para gestión integral de leads y comunicaciones en tiempo real para Krak Real Estate.

> ⚠️ **IMPORTANTE**: Este repositorio contiene configuraciones sensibles. Lee [SECURITY.md](./SECURITY.md) antes de hacer commit o compartir archivos.

---

## 🔐 Guías de Configuración

- **[SECURITY.md](./SECURITY.md)** - Guía de seguridad y credenciales
- **[CREDENTIALS-MAP.md](./CREDENTIALS-MAP.md)** - Mapeo de variables de entorno
- **.env.example** - Template de configuración (ver `.env` para valores reales)

---

## 📋 Resumen de Automatizaciones

### 1. **Obtención de Leads de Landings**
**Propósito:** Hub centralizado de ingesta que captura leads desde múltiples landing pages de proyectos.

**Flujo Principal:**
- Recibe datos vía Webhooks desde diferentes landings
- Normaliza campos heterogéneos bajo esquema común
- Asigna agentes mediante Round Robin según vertical del lead.
- Registra leads en Google Sheets
- Notifica sobre nuevas fuentes o errores

**Integraciones:** n8n Webhook, Google Sheets, SMTP

---

### 2. **Obtención de Leads Facebook**
**Propósito:** Extrae leads de formularios de Facebook Lead Ads en tiempo real.

**Flujo Principal:**
- Consulta API de Facebook Graph
- Obtiene formularios activos y leads pendientes
- Normaliza datos y registra en base centralizada
- Asigna agentes mediante Round Robin según vertical del lead.
- Implementa control anti-duplicados

**Integraciones:** Facebook Graph API, Google Sheets, SMTP

---

### 3. **Twilio VoiceMail**
**Propósito:** Gestiona eventos de voz desde Twilio (mensajes de buzón y llamadas finalizadas).

**Flujo Principal:**
- Recibe webhooks de eventos de llamadas
- Mapea números telefónicos a agentes
- Asigna mensajes de voz mediante Round Robin si no fue atendida
- Descarga audio y envía notificación por email con adjunto

**Características:**
- Filtro de calidad: descarta audios menores a 3 segundos
- Distingue entre voicemail y llamadas respondidas
- Asignación automática por vertical (Administración, Ventas)

**Integraciones:** Twilio API, Google Sheets, SMTP

---

### 4. **Whatsapp Agent Industrial**
**Propósito:** AI Agent conversacional especializado en vertical industrial (naves, depósitos, terrenos).

**Flujo Principal:**
- Recibe mensajes desde WhatsApp vía Wasender
- Mantiene memoria conversacional con Window Buffer
- Califica leads mediante IA (identifica interés y presupuesto)
- Asigna automáticamente a asesores mediante Round Robin cuando lead está completo

**Características:**
- Inteligencia Artificial con contexto persistente
- Validación anti-duplicados con Redis
- Logging automático de conversaciones
- Gestión centralizada de errores

**Integraciones:** Wasender Webhook, IA/LLM, Redis, Google Sheets, SMTP

---

### 5. **Whatsapp Agent Principal**
**Propósito:** AI Agent general de atención al cliente para consultas diversas sobre propiedades.

**Flujo Principal:**
- Recibe mensajes de WhatsApp (entrada general)
- Mantiene contexto conversacional con memoria persistente
- Califica leads conversacionalmente
- Asigna agentes mediante Round Robin según vertical del lead.

**Características:**
- Asistente oficial de Krak RE vía WhatsApp
- Manejo de conversaciones coherentes y naturales
- Resumen automático para handoff a agentes humanos
- Gestión de errores centralizada

**Integraciones:** Wasender Webhook, IA/LLM, Redis, Google Sheets, SMTP

---

## 🔄 Flujo de Procesamiento General

```
Fuente de Lead (Landing/Facebook/Teléfono/WhatsApp)
    ↓
Webhook → Normalización
    ↓
Validación & Anti-Duplicados
    ↓
Calificación (Manual o IA)
    ↓
Asignación Round Robin
    ↓
Registro en CRM/Sheets
    ↓
Notificación al Agente
```

---

## 📊 Integraciones Principales

| Servicio | Uso | Archivos |
|----------|-----|---------|
| **Google Sheets** | Base de datos centralizada de leads y agentes | Todos |
| **Twilio** | Llamadas y voicemail | Twilio VoiceMail |
| **Facebook Graph API** | Extracción de leads de anuncios | Obtención de Leads Facebook |
| **Wasender** | Webhooks de WhatsApp | Whatsapp Agents |
| **Redis** | Buffers anti-duplicados y memoria de sesión | Whatsapp Agents |
| **SMTP (Hostinger)** | Notificaciones por email | Todos |
| **IA/LLM** | Procesamiento conversacional y calificación | Whatsapp Agents |

---

## ⚙️ Configuración Técnica

### Variables de Entorno Requeridas
- Credenciales de Google Sheets
- Tokens de Facebook Graph API
- Credenciales de Twilio
- Configuración de Wasender Webhooks
- Credenciales SMTP
- Conexión a Redis
- API Key del LLM

---

## 🔐 Seguridad & Calidad

- ✅ Validación anti-duplicados con Redis
- ✅ Filtros de calidad de datos
- ✅ Gestión centralizada de errores
- ✅ Notificaciones de fallos en tiempo real
- ✅ Auditoría en Google Sheets
- ✅ Credenciales aisladas por servicio

---

## 📈 Métricas Clave

- Tasa de leads procesados por fuente
- Tiempo de asignación a agente
- Tasa de calificación automática
- Distribución de carga mediante Round Robin
- Tasa de errores y recuperación

---

## 📞 Contacto & Soporte

Para consultas sobre estas automatizaciones, revisar la documentación técnica dentro de cada workflow (sticky notes en N8N).
