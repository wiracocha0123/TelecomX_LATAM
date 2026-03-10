
# Informe final

> **Objetivo:** Identificar los factores que influyen en el abandono de clientes de TelecomX y proponer recomendaciones estratégicas para reducirlo.

---

## 1. Descripción del Dataset

| Característica | Detalle |
|---|---|
| Registros originales | 7.267 |
| Registros tras limpieza | 7.032 |
| Variables analizadas | 22 |
| Variable objetivo | `Abandonó_Servicio` |

El dataset fue extraído en formato JSON anidado y normalizado en columnas planas. Se eliminaron 235 registros por valores vacíos en `Churn` y valores no numéricos en `Charges.Total`. No se encontraron duplicados.

---

## 2. Proceso ETL

### Extracción
- Fuente: repositorio GitHub (Alura Cursos)
- Formato original: JSON con columnas anidadas (`customer`, `phone`, `internet`, `account`)

### Transformación
- Desanidado de columnas JSON con `pd.json_normalize`
- Eliminación de registros con `Churn` vacío y `Charges.Total` no numérico
- Conversión de variables binarias (`Yes`/`No`) a enteros (1/0)
- Traducción de columnas y valores al español
- Creación de variable derivada `Cargo_Diario` = `Cargo_Mensual / 30`
- Reset de índice para integridad del DataFrame

---

## 3. Análisis Descriptivo

### Tasa de Abandono General

```
Clientes que permanecieron:  73.42%
Clientes que abandonaron:    26.58%  
```
> **1 de cada 4 clientes abandona el servicio.** Esta tasa supera el umbral crítico del 20%, requiriendo atención inmediata.

### Estadísticas Clave de Variables Numéricas

| Variable | Media | Rango |
|---|---|---|
| Antigüedad | 32 meses | 1 — 72 meses |
| Cargo Mensual | $64.80 | $18.25 — $118.75 |
| Cargo Total | $2,283 | $18.80 — $8,684 |
| Cargo Diario | $2.16 | $0.61 — $3.96 |

### Adopción de Servicios

| Servicio | Adopción |
|---|---|
| Servicio Telefónico | 90.33% |
| Facturación Sin Papel | 59.27% |
| Streaming (TV/Películas) | ~38–39% |
| Seguridad / Soporte / Protección | 28–34% |
| Clientes Senior | 16.24% |

---

## 4. Análisis por Variables Categóricas

### Factores de Alto Riesgo (Churn > 40%)

| Factor | Tasa de Churn |
|---|---|
| Pago con Cheque Electrónico | 45.3% |
| Contrato Mensual | 42.7% |
| Servicio de Fibra Óptica | 41.9% |
| Cliente Senior | 41.7% |

### Factores de Riesgo Moderado (Churn 30–35%)

| Factor | Tasa de Churn |
|---|---|
| Facturación Sin Papel | 33.6% |
| Sin Pareja | 33.0% |
| Sin Dependientes | 31.3% |

### Factores de Bajo Riesgo (Churn < 20%)

| Factor | Tasa de Churn |
|---|---|
| Contrato Bienal | 2.8% |
| Sin Servicio de Internet | 7.4% |
| Contrato Anual | 11.3% |
| Con Dependientes | 15.5% |
| Servicio DSL | 18.9% |

> **El género no representa un factor diferencial** (Femenino 27.0% vs Masculino 26.2%).

---

## 5. Análisis por Variables Numéricas

### Antigüedad vs Churn
- Los clientes **nuevos (0–18 meses)** presentan la **mayor tasa de abandono**
- A medida que aumenta la antigüedad, el churn disminuye significativamente
- Los clientes con más de 54 meses tienen tasas de abandono mínimas

### Cargos Totales vs Churn
- Los clientes que abandonan concentran sus cargos totales en valores **bajos**, indicando que se van **antes de acumular historial de pago**
- Los clientes que permanecen muestran una distribución más amplia de cargos totales

---

## 6. Análisis de Correlaciones con Churn

### Correlaciones más relevantes con `Abandonó_Servicio`

| Variable | Correlación | Dirección |
|---|---|---|
| Antigüedad | -0.354 | 🔽 A mayor antigüedad, menor churn |
| Cargo Total | -0.199 | 🔽 A mayor gasto acumulado, menor churn |
| Facturación Sin Papel | +0.191 | 🔼 Asociado a mayor churn |
| Cargo Mensual | +0.193 | 🔼 Mayor cargo mensual, mayor riesgo |
| Tiene Dependientes | -0.163 | 🔽 Con dependientes, menor churn |
| Seguridad Online | -0.171 | 🔽 Con seguridad, menor churn |
| Soporte Técnico | -0.165 | 🔽 Con soporte, menor churn |
| Cliente Senior | +0.151 | 🔼 Clientes senior, mayor riesgo |

> **La antigüedad es el predictor más fuerte del churn** (r = -0.354). Retener al cliente en los primeros 18 meses es crítico.

---

## 7. Conclusiones y Recomendaciones

### Perfil de Mayor Riesgo
> Cliente con **contrato mensual + fibra óptica + pago por cheque electrónico + sin vínculos familiares + antigüedad menor a 18 meses**

### Perfil de Menor Riesgo
> Cliente con **contrato bienal o anual + con pareja o dependientes + DSL + alta antigüedad**

### Recomendaciones Estratégicas

1. **Incentivar contratos largos:** Ofrecer descuentos o beneficios para migrar de mensual a anual/bienal, dado que el churn cae de 42.7% a 11.3% y 2.8% respectivamente.

2. **Atención especial en los primeros 18 meses:** Implementar programas de onboarding y seguimiento activo para reducir la fuga temprana.

3. **Revisar la oferta de Fibra Óptica:** Con un 41.9% de churn, es necesario evaluar si los precios, la calidad o el soporte técnico están generando insatisfacción.

4. **Fomentar servicios de valor agregado:** Clientes con Seguridad Online y Soporte Técnico presentan menor churn; promover su adopción puede mejorar la retención.

5. **Estrategia diferenciada para clientes senior:** Representan el 16.24% de la base pero tienen un churn del 41.7%; se recomienda atención personalizada y planes adaptados.

6. **Revisión del método de pago:** Migrar clientes del cheque electrónico (45.3% churn) hacia métodos automáticos puede reducir la fricción en el pago y mejorar la retención.
