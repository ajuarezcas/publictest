# SquidProxy Security Copilot Plugin - Documentación

## Descripción General

El **SquidProxy Security Copilot Plugin** es una solución KQL personalizada que permite realizar consultas en lenguaje natural sobre logs de SquidProxy_CL almacenados en Microsoft Sentinel. El plugin utiliza la potencia nativa de KQL para proporcionar análisis completos, parseo automático de datos y insights de seguridad.

## Características Principales

### 🔍 Consultas en Lenguaje Natural
- Convierte preguntas en español/inglés a consultas KQL optimizadas
- Parseo automático de campos embebidos en la columna RawData
- Conversión automática de timestamps epoch a fechas legibles

### 📊 Análisis Integrado con KQL
- Detección de patrones temporales y anomalías usando KQL nativo
- Análisis de tráfico por cliente IP y dominio
- Identificación de errores y comportamientos sospechosos
- Correlación entre múltiples eventos y métricas
- Filtros dinámicos y consultas adaptativas

### 🛡️ Capacidades de Seguridad
- Detección de IPs con alta tasa de errores
- Identificación de tráfico a dominios externos vs Microsoft
- Análisis de conexiones fallidas y errores de acceso
- Monitoreo de patrones de acceso inusuales

## Arquitectura y Dependencias

### 🏗️ **Plugin Autocontenido y Simplificado**

El plugin está implementado como **UN SOLO ARCHIVO KQL** sin dependencias externas:

#### 📁 **Archivo Principal**
- **`SquidProxyPlugin.yaml`** - Plugin KQL completo y funcional

#### ✅ **Dependencias de Infraestructura (Requeridas)**
- **Microsoft Security Copilot** - Plataforma host del plugin
- **Microsoft Sentinel** - Fuente de datos con tabla SquidProxy_CL
- **Azure AD Tenant** - Autenticación (heredada de Security Copilot)

#### 📊 **Estructura de Datos Requerida**
```sql
-- Tabla SquidProxy_CL en Microsoft Sentinel:
SquidProxy_CL
├── TimeGenerated (datetime)     -- ✅ Requerido
├── RawData (string)            -- ✅ CRÍTICO - Datos parseables
└── Type (string)               -- ✅ "SquidProxy_CL"
```

### 🎯 **Capacidades del Plugin KQL**

- **Función**: Consultas directas a Microsoft Sentinel usando KQL nativo
- **Capacidades Técnicas**: 
  - Parseo completo de RawData con extracción de 10+ campos
  - Conversión automática de timestamps epoch a datetime
  - Clasificación inteligente de tipos de conexión
  - Detección automática de dominios Microsoft vs externos
  - Filtros dinámicos y consultas adaptativas
  - **Sin dependencias externas** - Solo funciones KQL nativas

- **4 Skills Especializados**:
  - `AnalyzeSquidProxyEvents`: Análisis general con filtros dinámicos
  - `AnalyzeSquidProxyErrors`: Detección específica de errores y anomalías  
  - `AnalyzeClientTraffic`: Análisis detallado por IP cliente
  - `AnalyzeDomainActivity`: Análisis por dominio y URL

## Ejemplos de Uso

### Consultas Básicas con Lenguaje Natural

```natural
"¿Cuántos eventos SquidProxy_CL ocurrieron el 15 de octubre de 2025?"
```
**Resultado**: El plugin ejecuta una consulta KQL filtrada por fecha y retorna el conteo total.

```natural
"Muestra los eventos donde la IP de origen es 10.60.0.10 en las últimas 24 horas"
```
**Resultado**: Lista de eventos filtrados por IP específica con todos los campos parseados.

```natural
"Filtra los eventos por tipo TCP_TUNNEL que tuvieron errores"
```
**Resultado**: Eventos de túneles TCP que resultaron en códigos de error.

### Análisis Avanzado

```natural
"Analiza patrones sospechosos en las conexiones proxy de hoy"
```
**Resultado**: El plugin identifica IPs con alta tasa de errores, dominios inusuales y patrones anómalos.

```natural
"Genera un reporte ejecutivo del tráfico proxy de esta semana"
```
**Resultado**: Reporte con métricas de seguridad, rendimiento y uso, incluyendo gráficos y tendencias.

## 🚀 Instalación Simplificada

### ✅ **Prerrequisitos Mínimos**

| Requerimiento | Descripción | Estado |
|---------------|-------------|---------|
| **Microsoft Security Copilot** | Plataforma con permisos admin | ✅ Requerido |
| **Microsoft Sentinel** | Workspace con tabla SquidProxy_CL | ✅ Requerido |
| **Permisos Azure** | Lectura en Sentinel workspace | ✅ Requerido |

### 📁 **Archivos Necesarios**

#### **Configuración Mínima** (Solo funcionalidad):
```
SquidProxyPlugin/
└── SquidProxyPlugin.yaml  ← ÚNICO archivo crítico
```

#### **Configuración Recomendada** (Funcionalidad + Documentación):
```
SquidProxyPlugin/
├── SquidProxyPlugin.yaml    ← Plugin principal
├── README.md                ← Documentación (este archivo)
└── Ejemplos-de-Uso.md       ← Guía de casos de uso
```

### ⚡ **Instalación en 6 Pasos**

1. **Descargar**: Solo el archivo `SquidProxyPlugin.yaml`
2. **Abrir**: Microsoft Security Copilot
3. **Ir a**: **Manage Plugins** > **Custom** > **Add Plugin**
4. **Subir**: El archivo `SquidProxyPlugin.yaml`
5. **Configurar** 4 parámetros:
   - **TenantId**: `12345678-1234-1234-1234-123456789abc`
   - **SubscriptionId**: `12345678-1234-1234-1234-123456789abc`
   - **ResourceGroupName**: `rg-sentinel-prod`
   - **WorkspaceName**: `sentinel-workspace-prod`
6. **Activar**: Plugin y probar con consulta simple

### 🔐 **Configuración de Permisos**

```bash
# Permisos mínimos requeridos en Azure:
Microsoft.OperationalInsights/workspaces/query/read
Microsoft.OperationalInsights/workspaces/read
Microsoft.SecurityInsights/incidents/read
```

### 🧩 **Dependencias y Archivos del Proyecto**

#### ✅ **Archivos ESENCIALES para Funcionalidad**
| Archivo | Propósito | Necesidad |
|---------|-----------|-----------|
| `SquidProxyPlugin.yaml` | Plugin KQL principal | 🔴 **CRÍTICO** |

#### 🟡 **Archivos RECOMENDADOS para Operación**  
| Archivo | Propósito | Necesidad |
|---------|-----------|-----------|
| `README.md` | Documentación técnica | 🟡 **IMPORTANTE** |
| `Ejemplos-de-Uso.md` | Casos de uso prácticos | 🟡 **IMPORTANTE** |

#### ⚪ **Archivos OPCIONALES** 
| Archivo | Propósito | Necesidad |
|---------|-----------|-----------|
| `config.yaml` | Configuración avanzada | 🟢 **ÚTIL** |
| `PLUGIN-SUMMARY.md` | Resumen ejecutivo | 🟢 **ÚTIL** |

#### ✅ **Proyecto Completamente Limpio**

```bash
# ✅ SOLO archivos esenciales (7 archivos innecesarios eliminados):
SquidProxyPlugin/
├── SquidProxyPlugin.yaml    # 🔴 CRÍTICO - Plugin KQL principal (15.6 KB)
├── README.md                # 🟡 IMPORTANTE - Documentación técnica (11.5 KB)
├── Ejemplos-de-Uso.md       # 🟡 IMPORTANTE - Casos de uso (8.9 KB)
├── PLUGIN-SUMMARY.md        # 🟢 ÚTIL - Resumen ejecutivo (9.2 KB)
├── config.yaml              # 🟢 ÚTIL - Configuración opcional (6.3 KB)
└── REPLICATION-GUIDE.md     # 🔵 REPLICACIÓN - Guía para recrear (15.8 KB)

Total: 6 archivos, 67.2 KB (anteriormente 12 archivos, 2.2 MB)
Reducción: 50% menos archivos, 97% menos espacio
```

> 📋 **¿Quieres replicar este plugin?** Ver [`REPLICATION-GUIDE.md`](./REPLICATION-GUIDE.md) para instrucciones detalladas paso a paso.

## Arquitectura del Parseo de RawData

### Estructura Original del Log SquidProxy

```
TimeGenerated | RawData | Type
-------------|---------|------
2025-10-15... | "1760533045.543 942920 10.60.0.10 TCP_TUNNEL/200..." | SquidProxy_CL
```

### Campos Extraídos Automáticamente

El plugin parsea la columna `RawData` y extrae:

| Campo Original | Campo Parseado | Descripción | Ejemplo |
|---------------|----------------|-------------|---------|
| `ParsedData[0]` | `SquidDateTime` | Timestamp convertido de epoch | `2025-10-15 12:57:25` |
| `ParsedData[1]` | `Duration_ms` | Duración en milisegundos | `942920` |
| `ParsedData[2]` | `ClientIP` | Dirección IP del cliente | `10.60.0.10` |
| `ParsedData[3]` | `ResultStatus` | Estado de la conexión | `TCP_TUNNEL/200` |
| `ParsedData[4]` | `Bytes` | Bytes transferidos | `11160` |
| `ParsedData[5]` | `Method` | Método HTTP | `CONNECT` |
| `ParsedData[6]` | `URL` | Destino de la conexión | `microsoft.com:443` |
| `ParsedData[7]` | `User` | Usuario (si disponible) | `-` |
| `ParsedData[8]` | `Hierarchy` | Jerarquía del proxy | `HIER_DIRECT/1.2.3.4` |

### Clasificaciones Automáticas

El plugin añade clasificaciones inteligentes:

- **ConnectionType**: `HTTPS Tunnel`, `Cache Miss`, `Memory Cache Hit`, etc.
- **IsMicrosoftDomain**: `true`/`false` para dominios Microsoft
- **Domain**: Dominio extraído de la URL
- **IsError**: `true` para códigos de error (400, 403, 500, etc.)

## Extensibilidad Futura

### Funciones Plannedadas para v2.0

1. **Machine Learning**: Detección automática de anomalías usando ML
2. **Correlación Multi-tabla**: Cruzar datos con otras tablas de Sentinel
3. **Alertas Proactivas**: Configuración de alertas basadas en patrones
4. **Dashboards Interactivos**: Visualizaciones integradas en Security Copilot
5. **Integración con SOAR**: Automatización de respuestas a incidentes

### Personalización Avanzada

#### Añadir Nuevos Skills KQL

1. Edita el archivo `SquidProxyPlugin.yaml`
2. Añade un nuevo skill bajo `SkillGroups[0].Skills`
3. Define los inputs y el template KQL
4. Vuelve a subir el plugin

Ejemplo de nuevo skill:

```yaml
- Name: AnalyzeBandwidthUsage
  DisplayName: Analyze Bandwidth Usage
  Description: Analiza el uso de ancho de banda por cliente y destino
  Inputs:
    - Name: threshold_mb
      Description: Umbral en MB para considerar uso alto
      Required: false
      DefaultValue: "100"
  Settings:
    Target: Sentinel
    # ... configuración existente ...
    Template: |-
      SquidProxy_CL
      | extend ParsedData = split(RawData, " ")
      | extend Bytes = toint(ParsedData[4])
      | summarize TotalMB = sum(Bytes)/1024/1024 by ClientIP = tostring(ParsedData[2])
      | where TotalMB >= toint("{{threshold_mb}}")
      | order by TotalMB desc
```

#### Personalizar la API

1. Modifica `squidproxy_api.py` para añadir nuevos endpoints
2. Actualiza `openapi.yaml` con las nuevas especificaciones
3. Redespliega el servicio API

## Troubleshooting

### Problemas Comunes

#### Error: "No se pueden encontrar logs"
**Solución**: Verifica que la tabla `SquidProxy_CL` existe y tiene datos en el timeframe especificado.

#### Error: "Permisos insuficientes"
**Solución**: Asegúrate de que Security Copilot tiene permisos de lectura sobre el workspace de Sentinel.

#### Error: "Parseo de RawData falla"
**Solución**: Verifica el formato de los logs. El plugin espera el formato estándar de Squid Proxy.

### Logs y Debugging

Para debugging del plugin KQL, revisa:

1. Los logs de Security Copilot en el portal de administración
2. Los resultados de las consultas KQL directamente en Microsoft Sentinel
3. Los parámetros pasados a los templates KQL
4. La sintaxis y estructura de las consultas generadas
5. Los permisos de acceso a la tabla SquidProxy_CL

## Soporte y Contribuciones

Para soporte técnico o para contribuir mejoras:

1. **Documentación**: Mantén actualizada esta documentación
2. **Testing**: Prueba nuevas funcionalidades con datos reales
3. **Feedback**: Recopila feedback de usuarios finales
4. **Versioning**: Usa versionado semántico para releases

## Licencia y Compliance

Este plugin es desarrollado bajo las siguientes consideraciones:

- **Privacidad**: No almacena logs fuera del entorno de Sentinel
- **Seguridad**: Usa autenticación Azure AD nativa
- **Compliance**: Compatible con regulaciones de retención de datos
- **Auditoría**: Todas las consultas son auditables via Sentinel logs

---

**Versión**: 1.0 MVP  
**Fecha**: 15 de octubre de 2025  
**Autor**: Agustin Juarez  
**Próxima Revisión**: 15 de noviembre de 2025