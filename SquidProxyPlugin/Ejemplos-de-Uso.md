# Ejemplos de Uso del Plugin SquidProxy para Security Copilot
# Casos de uso reales y consultas en lenguaje natural
# Versión: 1.0 MVP

## CASOS DE USO BÁSICOS

### 1. Análisis General de Eventos
**Consulta Natural**: "¿Cuántos eventos SquidProxy_CL ocurrieron el 15 de octubre de 2025?"

**Resultado Esperado**: 
- Conteo total de eventos en la fecha especificada
- Desglose por horas del día
- Top IPs más activas
- Distribución por tipos de conexión

**Skill Utilizada**: `AnalyzeSquidProxyEvents`

---

### 2. Búsqueda por IP Específica
**Consulta Natural**: "Muestra los eventos donde la IP de origen es 10.60.0.10 en las últimas 24 horas"

**Resultado Esperado**:
- Lista de todas las conexiones de esa IP
- Destinos más frecuentes
- Tipos de conexión utilizados
- Errores detectados (si los hay)

**Skill Utilizada**: `AnalyzeClientTraffic`

---

### 3. Filtrado por Tipo de Conexión
**Consulta Natural**: "Filtra los eventos por tipo TCP_TUNNEL que tuvieron errores"

**Resultado Esperado**:
- Eventos TCP_TUNNEL con códigos de error
- IPs afectadas
- Timestamps de los errores
- Destinos que causaron errores

**Skill Utilizada**: `AnalyzeSquidProxyErrors`

---

## CASOS DE USO AVANZADOS

### 4. Detección de Patrones Sospechosos
**Consulta Natural**: "Identifica IPs con alta tasa de errores en las últimas 12 horas"

**Resultado Esperado**:
- IPs con más de 20% de tasa de errores
- Tipos de errores más comunes
- Correlación temporal de errores
- Recomendaciones de acción

**Skills Utilizadas**: `AnalyzeSquidProxyErrors` + `AnalyzeClientTraffic`

---

### 5. Análisis de Dominios Externos
**Consulta Natural**: "¿Qué dominios externos (no Microsoft) se acceden más frecuentemente?"

**Resultado Esperado**:
- Top 20 dominios externos más visitados
- Volumen de tráfico por dominio
- IPs que acceden a dominios externos
- Análisis de seguridad de dominios

**Skill Utilizada**: `AnalyzeDomainActivity`

---

### 6. Investigación de Incidentes de Seguridad
**Consulta Natural**: "Analiza todo el tráfico sospechoso del cliente 10.3.1.4 en el último día"

**Resultado Esperado**:
- Historial completo de conexiones
- Errores y fallos de conexión
- Intentos de acceso bloqueados  
- Timeline de actividad sospechosa

**Skills Utilizadas**: `AnalyzeClientTraffic` + `AnalyzeSquidProxyErrors`

---

## CONSULTAS DE MONITOREO CONTINUO

### 7. Dashboard de Salud del Proxy
**Consulta Natural**: "Dame un resumen ejecutivo del estado del proxy en las últimas 24 horas"

**Información Incluida**:
- Total de conexiones procesadas
- Tasa de errores global
- Top 10 clientes más activos
- Top 10 destinos más accedidos
- Distribución Microsoft vs externos
- Métricas de rendimiento (duración, bytes)

---

### 8. Alertas de Seguridad
**Consulta Natural**: "Detecta actividad anómala en las conexiones proxy de hoy"

**Patrones Detectados**:
- IPs con comportamiento inusual
- Acceso a dominios maliciosos conocidos
- Picos de tráfico fuera de horarios normales
- Conexiones fallidas masivas

---

### 9. Análisis de Rendimiento
**Consulta Natural**: "¿Cuáles son las conexiones más lentas y qué las causa?"

**Análisis Incluido**:
- Top conexiones por duración
- Correlación duración vs destino
- Análisis de cache hits vs misses
- Recomendaciones de optimización

---

### 10. Compliance y Auditoría
**Consulta Natural**: "Genera un reporte de todos los accesos externos para auditoría"

**Reporte Incluye**:
- Lista completa de dominios externos accedidos
- Usuarios/IPs que accedieron
- Frecuencia y volumen de acceso
- Clasificación de riesgo por dominio

---

## CONSULTAS ESPECÍFICAS POR TIMESTAMP

### 11. Análisis de Ventana Temporal Específica
**Consulta Natural**: "Analiza el tráfico proxy entre las 10:00 AM y 2:00 PM de hoy"

**KQL Generado**:
```kql
SquidProxy_CL
| extend ParsedData = split(RawData, " ")
| extend SquidDateTime = unixtime_seconds_todatetime(todouble(ParsedData[0]))
| where SquidDateTime >= datetime(2025-10-15 10:00:00) and 
        SquidDateTime <= datetime(2025-10-15 14:00:00)
| extend ClientIP = tostring(ParsedData[2])
| summarize Connections = count() by ClientIP, bin(SquidDateTime, 1h)
| order by SquidDateTime desc
```

---

### 12. Comparación Temporal
**Consulta Natural**: "Compara el tráfico de hoy vs ayer mismo horario"

**Análisis Comparativo**:
- Volumen de conexiones día a día
- Cambios en patrones de tráfico
- Nuevos dominios o IPs
- Variaciones en tasas de error

---

## CONSULTAS DE TROUBLESHOOTING

### 13. Diagnóstico de Conectividad
**Consulta Natural**: "¿Por qué fallan las conexiones a microsoft.com desde la IP 10.60.0.46?"

**Investigación Incluye**:
- Historial de conexiones exitosas vs fallidas
- Códigos de error específicos
- Comparación con otras IPs
- Timeline de problemas

---

### 14. Análisis de Bandwidth
**Consulta Natural**: "¿Qué clientes consumen más ancho de banda y a qué destinos?"

**Métricas Analizadas**:
- Total de bytes por cliente IP
- Destinos con mayor transferencia
- Promedio de bytes por conexión
- Identificación de uso excesivo

---

### 15. Investigación de Cache
**Consulta Natural**: "Analiza la efectividad del cache del proxy en la última semana"

**Estadísticas de Cache**:
- Ratio de cache hits vs misses
- Dominios más cacheados
- Impacto en rendimiento
- Recomendaciones de optimización

---

## CONSULTAS CON CORRELACIÓN AVANZADA

### 16. Análisis Multi-dimensión
**Consulta Natural**: "Correlaciona errores de conexión con horarios pico de actividad"

**Análisis Cruzado**:
- Distribución temporal de errores
- Correlación con volumen de tráfico
- Identificación de cuellos de botella
- Patrones de fallos recurrentes

---

### 17. Detección de Amenazas
**Consulta Natural**: "Busca patrones de exfiltración de datos o comunicación con C&C"

**Indicadores Monitoreados**:
- Transferencias grandes fuera de horarios
- Conexiones a dominios sospechosos
- Patrones de comunicación inusuales
- Correlación con listas de amenazas

---

### 18. Análisis Forense
**Consulta Natural**: "Reconstruye la actividad de red del incidente del 15 de octubre a las 13:30"

**Reconstrucción Incluye**:
- Timeline completo de eventos
- Todas las IPs involucradas
- Secuencia de conexiones
- Contexto de seguridad

---

## CONSULTAS DE REPORTING EJECUTIVO

### 19. KPIs de Seguridad
**Consulta Natural**: "Dame los KPIs de seguridad del proxy para el reporte mensual"

**KPIs Incluidos**:
- Tasa de errores global
- % de tráfico externo vs interno
- Top amenazas detectadas
- Incidentes de seguridad resueltos

---

### 20. ROI y Eficiencia
**Consulta Natural**: "Analiza la eficiencia operativa del proxy y su ROI"

**Métricas de Eficiencia**:
- Reducción de ancho de banda por cache
- Tiempo de respuesta promedio
- Disponibilidad del servicio
- Costos de bandwidth evitados

---

## PLANTILLAS DE CONSULTAS PERSONALIZABLES

### Plantilla para Nuevos Análisis
```natural
"Analiza [MÉTRICA] para [ENTIDAD] en [TIMEFRAME] donde [CONDICIÓN]"

Ejemplos:
- "Analiza errores para clientes VIP en las últimas 48 horas donde el tráfico > 100MB"
- "Analiza dominios para la subred 10.60.0.0/24 en octubre donde hubo conexiones fallidas"
```

### Plantilla para Investigaciones
```natural
"Investiga [EVENTO] relacionado con [IP/DOMINIO] desde [FECHA] hasta [FECHA]"

Ejemplos:
- "Investiga accesos anómalos relacionados con 10.3.1.4 desde ayer hasta hoy"
- "Investiga conexiones fallidas relacionadas con azure.com en la última semana"
```

---

## AUTOMATIZACIÓN Y ALERTAS

### Consultas Programadas
El plugin puede configurarse para ejecutar consultas automáticamente:

1. **Monitoreo cada hora**: "Detecta nuevas IPs con > 50 errores en la última hora"
2. **Reporte diario**: "Resumen de actividad de las últimas 24 horas"  
3. **Alerta semanal**: "Identifica cambios significativos en patrones de tráfico"

---

## INTEGRACIÓN CON WORKFLOWS DE SOC

### Investigación de Incidentes Tipo 1: Conexión Anómala
1. "¿Hay tráfico inusual desde [IP sospechosa]?"
2. "¿A qué dominios se conecta [IP sospechosa]?"
3. "¿Cuándo empezó este comportamiento anómalo?"
4. "¿Hay otras IPs con el mismo patrón?"

### Investigación de Incidentes Tipo 2: Exfiltración de Datos  
1. "¿Qué clientes transfirieron > 1GB en las últimas 24h?"
2. "¿A qué destinos externos se enviaron grandes volúmenes?"
3. "¿Hubo transferencias fuera del horario laboral?"
4. "¿Coincide con otros indicadores de compromiso?"

---

**Nota**: Todos estos ejemplos utilizan las capacidades del plugin para convertir lenguaje natural a consultas KQL optimizadas, parseando automáticamente los timestamps epoch y extrayendo información estructurada de la columna RawData.