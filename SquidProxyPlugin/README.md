# Squid Proxy Insights Plugin

Este plugin personalizado ofrece endpoints semánticos para consultar la tabla `SquidProxy_CL` en Microsoft Sentinel mediante Security Copilot.

## Objetivo
Reducir consultas KQL manuales recurrentes y entregar respuestas estructuradas y resumidas adecuadas para análisis rápido o enriquecimiento en prompts.

## Tabla origen
`SquidProxy_CL` con columnas:
- TimeGenerated (datetime)
- Computer (string)
- Source_s (string)
- DestinationIP_s (string)
- DestinationPort_d (int)
- User_s (string)
- RequestMethod_s (string)
- Url_s (string)
- StatusCode_d (int)
- BytesSent_d (int)
- BytesReceived_d (int)
- LogType (string)

## Intenciones de análisis cubiertas
1. Eventos recientes (listRecentProxyEvents)
2. Búsqueda por IP destino (searchProxyByClientIp)
3. Resumen de códigos HTTP (summarizeProxyStatusCodes)
4. Destinos más solicitados (topProxyDestinations)

## Autenticación
Se define `AADDelegated` para aprovechar el contexto del usuario en Security Copilot y aplicar RBAC sobre el workspace de Sentinel. No requiere Client ID explícito en el manifiesto del plugin. El usuario debe tener permisos de lectura (Log Analytics Reader / Sentinel Reader) sobre el workspace. 

## Parámetros clave
| Operación | Parámetros | Notas |
|-----------|-----------|-------|
| listRecentProxyEvents | minutesBack (default 60, max 1440), limit (default 50, max 200) | Lista ordenada por TimeGenerated desc |
| searchProxyByClientIp | ip (requerido), minutesBack (default 1440), limit (default 100, max 500) | Filtro por DestinationIP_s |
| summarizeProxyStatusCodes | minutesBack (default 1440), top (default 20, max 50) | Distribución de códigos HTTP |
| topProxyDestinations | minutesBack (default 1440), limit (default 20, max 100) | Agrega por DestinationIP_s |

## KQL (Plantillas)

```kql
// listRecentProxyEvents
SquidProxy_CL
| where TimeGenerated >= ago({minutesBack}m)
| top {limit} by TimeGenerated desc
| project TimeGenerated, Computer, Source_s, DestinationIP_s, DestinationPort_d, User_s, RequestMethod_s, Url_s, StatusCode_d, BytesSent_d, BytesReceived_d, LogType

// searchProxyByClientIp
SquidProxy_CL
| where TimeGenerated >= ago({minutesBack}m)
| where DestinationIP_s == "{ip}"
| top {limit} by TimeGenerated desc
| project TimeGenerated, Computer, DestinationIP_s, DestinationPort_d, User_s, RequestMethod_s, Url_s, StatusCode_d, BytesSent_d, BytesReceived_d

// summarizeProxyStatusCodes
SquidProxy_CL
| where TimeGenerated >= ago({minutesBack}m)
| summarize Count = count() by StatusCode_d
| order by Count desc
| top {top} by Count
| extend Percentage = (todouble(Count) * 100.0 / toscalar(SquidProxy_CL
    | where TimeGenerated >= ago({minutesBack}m)
    | summarize total = count()))

// topProxyDestinations
SquidProxy_CL
| where TimeGenerated >= ago({minutesBack}m)
| summarize requestCount = count(), bytesSentTotal = sum(BytesSent_d), bytesReceivedTotal = sum(BytesReceived_d) by DestinationIP_s
| top {limit} by requestCount desc
```

Notas de performance:
- Límites conservadores para evitar respuestas demasiado grandes.
- Se recomienda ajustar índices/enrichment si el volumen crece.

## Pasos para publicar
1. Alojar `openapi.yaml` en un repositorio accesible (raw URL HTTPS).
2. Editar `plugin.yaml` reemplazando `https://REEMPLAZAR_URL_HOSTED/openapi.yaml` por la URL real.
3. En Security Copilot: Plugins / Custom / Upload plugin → seleccionar formato Security Copilot plugin y subir `plugin.yaml`.
4. Completar setup (no se requiere API key) y habilitar el toggle del plugin.
5. Probar en un prompt: “Usa el plugin Squid Proxy Insights para listar eventos recientes de proxy de la última hora”.

## Mejoras futuras sugeridas
- Endpoint de búsqueda por usuario (`User_s`).
- Agregado de dominios extraídos del campo `Url_s`.
- Detección de anomalías (volumen por IP vs baseline 7d).
- Normalización de categorías (p.ej. clasificar 5xx vs 4xx vs 2xx en el summary).

## Troubleshooting
- 401/403: Verificar permisos RBAC del usuario sobre el workspace.
- Vacío: Confirmar que existan datos en el rango de tiempo consultado.
- Rendimiento lento: Reducir `minutesBack` o `limit`.

## Licencia
Puedes añadir una nota de licencia interna si corresponde.
