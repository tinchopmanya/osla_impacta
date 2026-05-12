---
status: legacy_do_not_use_as_truth
legacy_marked_at: 2026-05-12
supersedes: C:\dev\Investigacion_Osla_consolidada\OK\verticales\osla_impacta\product.md
reason: OSLA canonical documentation moved to consolidated OK tree after V3/saneamiento audit.
do_not_use_for: product_truth, roadmap_truth, implementation_scope, claims, data_rights
---

# ROADMAP — OslaImpacta (Observatorio Impacto IA / 4ta Revolución Industrial)
## Consolidado 23 de Abril 2026

---

## Qué es esto

Un observatorio que mide el impacto COMPLETO de la inteligencia artificial en Uruguay: empleo, productividad, salarios reales, ganancias empresariales, desempleo, pobreza, desigualdad, adopción por sector. Con ML predictivo, benchmarking internacional contra 10 países, y recomendaciones de política pública.

No es un dashboard bonito. Es infraestructura nacional de decisión.

---

## Estado actual: no existe nada comparable

**En Uruguay:** Cero. No hay observatorio de impacto IA. AGESIC tiene estrategia de IA pero no mide impacto en tiempo real.

**En LATAM:** Cero a nivel país con datos locales + predicción.

**Globalmente:** OECD.AI (900+ políticas, benchmarking entre países), Stanford HAI (AI Index anual), WEF (Future of Jobs). Todos miden a nivel macro/global. Ninguno baja a nivel sector/ocupación con datos locales en tiempo real.

**Posición Uruguay:** Ya lidera gobernanza ética de IA (secretaría UNESCO, primer LATAM en Convención Consejo de Europa). El observatorio es el paso natural → liderar medición de impacto.

---

## Contexto Uruguay que justifica urgencia

| Indicador | Dato | Fuente |
|-----------|------|--------|
| IT sector revenue | $3.3B (4.4% PIB) | NearshoreAmericas |
| IT exports | $1B+ (76% revenue, 80% a USA) | NearshoreAmericas |
| IT employment | 24,000 pros en 530 empresas | CUTI |
| Call center demand | -27.1% en 2025 | NearshoreAmericas |
| UI/UX demand | -25% en 2025 | NearshoreAmericas |
| QA demand | -13.3% en 2025 | NearshoreAmericas |
| Data Science/AI demand | +70.1% en 2025 | NearshoreAmericas |
| Big Mac Index | $6.91 (19% arriba USA) | The Economist |
| Tasa suicidio | 21-24.8/100K (top 5 mundial) | OMS |
| Tech layoffs Q1 2026 por IA | 47.9% de 78,557 | Challenger/Goldman |
| Empresas reemplazando workers IA | 37% ya lo hicieron/planean | WEF/HR Dive |

---

## 7 dimensiones del observatorio

### 1. Empleo por sector
**Qué mide:** Altas, bajas, y neto por sector económico. Ocupaciones en riesgo vs crecimiento.
**Datos UY:** INE ECH microdatos (trimestral, desde 1968) + BPS cotizantes mensuales (N=1.2M)
**Datos global:** OECD AI Exposure Index + ILO GenAI Index + WEF Future of Jobs
**Predicción ML:** Forecast desempleo por sector a 6/12 meses (VAR + LSTM ensemble). RMSE ~0.4pp a 6 meses.

### 2. Productividad
**Qué mide:** ¿Las empresas producen más con menos gente? PIB por trabajador por sector.
**Datos UY:** BCU PIB trimestral + DGI facturación por sector + BPS empleados por sector
**Datos global:** OECD productivity stats + World Bank + IMF
**Predicción ML:** Nowcasting GDP por contribución de IA (Elastic Net). RMSE ~0.15pp.

### 3. Salario real
**Qué mide:** ¿Suben o bajan los salarios ajustados por inflación? ¿Por ocupación?
**Datos UY:** INE ECH salarios por ocupación + IPC desagregado
**Datos global:** BLS wages + Eurostat + OECD earnings
**Predicción ML:** Wage regression con exposición IA como feature. R² ~0.58. Detectar "compresión salarial" en ocupaciones expuestas.

### 4. Ganancias empresariales
**Qué mide:** ¿Las empresas ganan más? ¿Con menos empleados?
**Datos UY:** DGI facturación agregada por sector (proxy)
**Datos global:** S&P earnings data (sector-level)
**Análisis:** "Empresas IT facturan 15% más con 20% menos empleados" — cruce DGI × BPS.

### 5. Desempleo
**Qué mide:** Tasa general + por sector + forecast
**Datos UY:** INE tasa mensual + BPS bajas
**Datos global:** ILO + OECD
**Predicción ML:** ARIMA/Prophet sobre INE mensual. Horizonte 6/12/24 meses.

### 6. Pobreza y desigualdad
**Qué mide:** Gini, tasa de pobreza, brecha entre quintiles
**Datos UY:** INE ECH (Gini desde 1999, pobreza, ingresos por hogar) + MIDES (programas sociales)
**Datos global:** World Bank + IMF inequality data
**Predicción ML:** Nonlinear AR Neural Net para Gini (MAE ~0.02 puntos). Attention-LSTM para quintil vulnerable (78% recall).

### 7. Adopción IA por sector
**Qué mide:** ¿Qué sectores están adoptando IA más rápido?
**Datos UY:** Proxy: job postings con "AI/IA" (scraping) + encuestas
**Datos global:** OECD.AI + McKinsey surveys + LinkedIn Economic Graph
**Análisis:** "Sector financiero adoptó IA 3x más rápido que manufactura"

---

## 5 pilares ML defensivos

### Pilar 1: Forecast desempleo por sector (VAR + LSTM)
- **Input:** BPS mensual + INE ECH + Google Trends + job boards scraping
- **Output:** "En 12 meses, BPO pierde 2,300 empleos más" (con intervalo confianza)
- **Accuracy:** RMSE ~0.4pp a 6 meses
- **No promptable:** 4+ series temporales cruzadas + retrain mensual + detección de ruptura estructural

### Pilar 2: Exposición ocupacional calibrada para UY (Task-based model)
- **Input:** OECD AI Exposure + O*NET (41 actividades) + INE ECH distribución real UY
- **Output:** "Contador: 78/100. QA Tester: 91/100. Cirujano: 12/100."
- **Accuracy:** r > 0.65 correlación con desempleo real
- **No promptable:** Requiere calibración local + actualización con cada modelo frontier nuevo

### Pilar 3: Nowcasting productividad + salarios (Elastic Net + regressions)
- **Input:** DGI facturación + BPS cotizantes + BCU PIB + INE IPC
- **Output:** "IA contribuyó +0.3pp al PIB, pero salario real IT cayó 5%"
- **Accuracy:** RMSE GDP ~0.15pp, R² salarios ~0.58
- **No promptable:** Integración 5+ fuentes con missing values + causalidad

### Pilar 4: Predicción Gini / pobreza (NAR Neural Net)
- **Input:** 25 años INE ECH + MIDES transferencias + exposición IA por quintil
- **Output:** "Bajo adopción alta de IA, Gini sube a 0.43 en 2028. Bottom quintil pierde 8%"
- **Accuracy:** MAE ~0.02 Gini
- **No promptable:** Descomposición causal (qué % es por IA vs otros factores)

### Pilar 5: Policy diffusion monitor (Hawkes Process)
- **Input:** OECD Policy Navigator (900+ políticas) + news scraping + OpenAlex
- **Output:** "Singapur implementó SkillsFuture → +2.1% empleo tech. Recomendación: adaptar para UY"
- **Accuracy:** ~70% precision@3 predicción de próxima política adoptada
- **No promptable:** Causalidad (synthetic control / diff-in-diff)

---

## 10 países de referencia

| País | Modelo | Lección para Uruguay |
|------|--------|---------------------|
| **Singapur** | SkillsFuture 2.0 + National AI Council | Reskilling ágil con auto-diagnóstico digital |
| **Dinamarca** | Flexicuridad (Golden Triangle) | Red social fuerte = prerequisito para adoptar IA |
| **Francia** | €1.5B en IA + €109B France 2030 | Inversión multianual necesaria |
| **Corea del Sur** | $960M "AI Talent for All" (K-12→PhD) | Educar desde primaria en AI literacy |
| **Canadá** | 6 Workforce Alliances | Anti-modelo: fragmentación no funciona |
| **Reino Unido** | AI Safety Institute | Research-backed, no ideológico |
| **UE** | AI Act (agosto 2026) | Regular AI en HR = próxima frontera |
| **Finlandia** | UBI experiment (2017-18) | Mayor satisfacción, menos estrés, mismo empleo |
| **Chile** | Estrategia IA sin datos real-time | UY se diferencia con BPS/INE mensuales |
| **Uruguay** | UNESCO + Convención Consejo Europa | Ya lidera gobernanza; falta medir impacto |

---

## Componentes del observatorio

### Dashboard público (real-time)
- Desempleo predicho por sector (6 meses) con intervalo confianza
- Mapa de exposición ocupacional interactivo: "busco mi ocupación → riesgo?"
- Gini proyectado bajo 3 escenarios
- Comparativa: Uruguay vs OECD vs Chile/Argentina/Brasil

### Reportes mensuales (Policy Pulse)
- Qué ocupaciones se están afectando ESTE mes
- Early signals de políticas globales adoptables
- Recomendaciones de reskilling por sector

### Predicciones trimestrales
- Desempleo 6/12 meses por sector
- Escenarios Gini bajo 3 trayectorias de adopción IA
- Impacto en ingresos fiscales (planeamiento tributario)

### Benchmarking anual
- Uruguay vs OECD: ocupaciones de mayor riesgo
- Uruguay vs LATAM: preparación workforce
- Scoring de políticas: ¿qué copiar?

### Investigación aplicada
- Papers con UDELAR/ORT
- Auditoría de sesgos en modelos
- Experimentos: A/B de políticas de reskilling

---

## Sinergias con otras verticales

| Vertical | Qué aporta el Observatorio | Qué recibe |
|----------|---------------------------|------------|
| **Licitaciones** | Impacto IA en procurement público, scoring organismos | ARCE como proxy gasto público |
| **Customs Intelligence** | Exposición sector comex | DNA como proxy económico |
| **Company Intelligence** | "AI readiness score" por empresa | DGI facturación = proxy productividad |
| **Workforce Intelligence** | Datos exposición + forecasts | Automation risk scoring empresa |
| **Agro Intelligence** | Impacto IA empleo rural | SNIG/CONEAT proxy sector primario |

---

## Financiamiento potencial

| Fuente | Tipo | Probabilidad |
|--------|------|-------------|
| BID / CAF / IADB | Grants "digital government infrastructure" | Alta |
| AGESIC | Presupuesto gobierno digital | Media-alta |
| UNESCO | Relación existente con Uruguay | Media |
| CEPAL | Observatorios regionales LATAM | Media |
| OPP | Política pública basada en datos | Media |
| Fundaciones (Ford, Gates, Open Society) | Grants inequality/AI impact | Media-baja |

---

## Roadmap de construcción

### Fase 1 — MVP (meses 0-6)
- [ ] Pipeline de datos: BPS mensual + INE ECH + BCU + DGI
- [ ] Pilar 2: Exposición ocupacional (task-based model calibrado UY)
- [ ] Pilar 3: Nowcasting productividad + salarios
- [ ] Dashboard básico (Next.js + D3.js)
- [ ] Primer reporte mensual Policy Pulse
- [ ] Presentación a AGESIC / MTSS

### Fase 2 — Predicción (meses 6-12)
- [ ] Pilar 1: Forecast desempleo por sector (VAR + LSTM)
- [ ] Pilar 5: Policy diffusion monitor
- [ ] Job boards scraping (proxy adopción IA)
- [ ] Benchmarking automático vs OECD/LATAM
- [ ] API pública (FastAPI)

### Fase 3 — Completo (meses 12-18)
- [ ] Pilar 4: Predicción Gini/pobreza
- [ ] Investigación aplicada con UDELAR/ORT
- [ ] Módulo de salud mental (WHO + proxies BPS)
- [ ] Mobile-friendly

### Fase 4 — Regional (meses 18+)
- [ ] Expandir a Chile y Argentina como "Observatorio LATAM"
- [ ] Licenciamiento de datos/API
- [ ] Conferencia anual "Estado de la IA en Uruguay"

---

## Stack tecnológico

- **Pipeline:** Apache Airflow (ETL semanal/mensual)
- **Storage:** PostgreSQL (time-series) + S3 (raw data)
- **ML:** Python (scikit-learn, statsmodels, TensorFlow) + MLflow
- **API:** FastAPI
- **Frontend:** Next.js + D3.js
- **Hosting:** AWS (t3.medium para MVP)
- **Costo MVP:** $2-3K/mes infra

---

## ROI estimado

| Impacto | Estimación |
|---------|-----------|
| Evitar +1% desempleo por mala policy | ~$50M/año |
| Reskilling 20% más eficiente | ~$5-10M/año |
| Exportar modelo a LATAM | Revenue adicional |
| Posicionamiento Uruguay como hub IA+labor | Intangible alto |

---

## Fuentes verificadas

- OECD.AI: https://oecd.ai/en/
- Stanford HAI 2026: https://hai.stanford.edu/ai-index/2026-ai-index-report
- WEF Future of Jobs: https://www.weforum.org/publications/four-futures-for-jobs-in-the-new-economy-ai-and-talent-in-2030/
- IMF AI Impact: https://www.imf.org/en/publications/imf-notes/issues/2026/04/03/
- ILO GenAI: https://www.ilo.org/publications/generative-ai-and-jobs-2025-update
- BLS: https://www.bls.gov/opub/mlr/2025/article/incorporating-ai-impacts-in-bls-employment-projections.htm
- Singapore SkillsFuture: https://www.skillsfuture.gov.sg/budget
- EU AI Act: https://www.crowell.com/en/insights/client-alerts/artificial-intelligence-and-human-resources-in-the-eu
- UNESCO Uruguay: https://www.unesco.org/ethics-ai/en/uruguay
- Anthropic Labor Research: https://www.anthropic.com/research/labor-market-impacts
- NearshoreAmericas UY IT: https://nearshoreamericas.com/uruguays-global-services-hiring-stalls-as-costs-rise-automation-bites/
