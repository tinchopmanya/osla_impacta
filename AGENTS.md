# AGENTS.md — OslaImpacta

## Identidad del proyecto

- **Nombre**: OslaImpacta (Observatorio Impacto IA / 4ta Revolución Industrial)
- **Ranking InvestigaVert**: (Combinado — Pick de Martin, fusiona "Future of Work" 67pts + "Observatorio Empleo" 62pts)
- **AshRise project_id**: `observatorio`
- **Puerto Postgres**: 5460
- **DB**: observatorio / user: observatorio
- **ICP primario**: Gobierno (OPP, MTSS, MIDES, AGESIC)
- **ICP secundario**: Universidades, BID/CAF/UNESCO, medios, empresas grandes (workforce planning)
- **Pricing target**: Grants BID/CAF + USD 500-2000/mes empresas grandes
- **Estado validación**: PENDIENTE — requiere validación con OPP/MTSS

## Qué es

Infraestructura nacional que mide impacto COMPLETO de IA en Uruguay: empleo por sector, productividad, salarios reales, ganancias empresariales, desempleo, pobreza, desigualdad, adopción IA. 5 pilares ML + benchmarking 10 países + policy recommendations. Dashboard público en tiempo real. Reportes mensuales "Policy Pulse" + predicciones trimestrales + benchmarking anual.

## Problema que resuelve

Uruguay lidera gobernanza ética de IA (secretaría UNESCO) pero no mide impacto en tiempo real. Gobiernos necesitan saber: "¿cuántos empleos pierde IA en BPO en 12 meses?" para policy correcta. Un observatorio que integra BPS + INE + DGI + MTSS mensualmente, predice desempleo 6/12 meses, y recommends políticas comparables a Singapur/Dinamarca.

## Modelos ML defensibles

### Pilar 1: Forecast desempleo por sector (VAR + LSTM)
- Input: BPS mensual (1.2M cotizantes) + INE ECH trimestral + job boards scraping
- Output: "BPO pierde 2,300 empleos en 12 meses" con intervalo confianza
- Accuracy: RMSE ~0.4pp a 6 meses
- No promptable: 4+ series temporales cruzadas + retrain mensual

### Pilar 2: Exposición ocupacional calibrada para UY (Task-based)
- Input: OECD AI Exposure (41 actividades) + O*NET + INE ECH distribución local
- Output: "Contador: 78/100. QA: 91/100. Cirujano: 12/100."
- Accuracy: r > 0.65 correlación con desempleo real
- No promptable: Calibración local + actualización con modelos frontier nuevos

### Pilar 3: Nowcasting productividad + salarios (Elastic Net)
- Input: DGI facturación + BPS cotizantes + BCU PIB + INE IPC
- Output: "IA contribuyó +0.3pp al PIB, pero salario real IT cayó 5%"
- Accuracy: RMSE GDP ~0.15pp, R² salarios ~0.58
- No promptable: 5+ fuentes con missing values + causalidad

### Pilar 4: Predicción Gini / pobreza (NAR Neural Net)
- Input: 25 años INE ECH + MIDES transferencias + exposición IA por quintil
- Output: "Bajo adopción alta, Gini sube a 0.43 en 2028"
- Accuracy: MAE ~0.02 Gini
- No promptable: Descomposición causal IA vs otros factores

### Pilar 5: Policy diffusion monitor (Hawkes Process)
- Input: OECD Policy Navigator (900+ políticas) + news scraping + OpenAlex
- Output: "Singapur SkillsFuture → +2.1% empleo tech. Adaptar para UY?"
- Accuracy: ~70% precision@3 en predicción próxima política
- No promptable: Causalidad vía synthetic control / diff-in-diff

## 10 países de referencia

| País | Modelo | Lección para Uruguay |
|------|--------|---------------------|
| Singapur | SkillsFuture 2.0 + National AI Council | Reskilling ágil con auto-diagnóstico digital |
| Dinamarca | Flexicuridad (Golden Triangle) | Red social fuerte = prerequisito |
| Francia | €1.5B IA + €109B France 2030 | Inversión multianual necesaria |
| Corea | $960M "AI Talent for All" | Educar desde primaria |
| Canadá | 6 Workforce Alliances | Anti-modelo: fragmentación no funciona |
| UK | AI Safety Institute | Research-backed, no ideológico |
| UE | AI Act (agosto 2026) | Regular AI en HR = próxima frontera |
| Finlandia | UBI experiment 2017-18 | Mayor satisfacción, mismo empleo |
| Chile | Estrategia IA (sin datos real-time) | UY se diferencia con BPS/INE mensual |
| Uruguay | UNESCO + Convención Consejo Europa | Ya lidera gobernanza; medir impacto |

## Fuentes de datos

### Uruguay
- INE ECH microdatos: desde 1968, trimestral, 10K+ encuestas
- BPS cotizantes mensuales: N=1.2M, estado laboral, actividad
- DGI: facturación agregada por sector
- BCU: PIB trimestral, cotizaciones
- MTSS: datos laborales, normativa
- MIDES: programas sociales, transferencias
- ANEP: educación, empleo por sector educativo
- Job boards scraping: LinkedIn, BumeranJob

### Internacionales
- OECD.AI: 900+ políticas, benchmarking
- ILO GenAI Index: probabilidad automatización
- WEF Future of Jobs: encuestas 800+ empresas
- IMF: datos macro, previsiones
- World Bank: desigualdad, pobreza
- Stanford HAI: research AI impact
- BLS: occupational projections
- Eurostat: datos comparables LATAM
- OpenAlex: research papers adoption

## Acceso a datos compartidos (via FDW)

```sql
SELECT * FROM shared.entity WHERE ...;
SELECT * FROM shared.fx_rates WHERE ...;
```

## Stack técnico

- Backend: FastAPI (Python)
- Frontend: Next.js 15 + D3.js
- DB: Postgres 16 (puerto 5460) + TimescaleDB
- ML: scikit-learn + statsmodels (VAR) + PyTorch (LSTM) + TensorFlow (NAR) + Hawkes
- Pipeline: Apache Airflow (ETL semanal/mensual)
- Storage: S3-compatible + Cloud-Optimized GeoTIFFs
- Visualization: D3.js + Plotly + Mapbox
- Hosting: AWS (t3.medium para MVP, $2-3K/mes)

## Reglas de boundary

1. Repo, base de datos, storage y colas propios.
2. No leer ni escribir directo en la base de otra vertical.
3. Compartir datos solo por FDW read-only desde shared-db.
4. Reusar patrones del núcleo reusable (source discovery, document extraction, identity resolution, alerts/tasks).

## Reglas para agentes

1. **No escribir en shared-db** — solo leer vía FDW
2. **BPS + INE ECH es fuente de verdad** para empleo y desigualdad
3. **Evidence-first**: cada predicción cita modelos, training data, RMSE, confianza
4. **Deterministic-first**: usar VAR antes que DL para series temporales
5. **No afirmar causalidad IA sin validación con control group sintético**

## Variables de entorno

```
ASHRISE_BASE_URL=http://localhost:8080
ASHRISE_TOKEN=<token>
ASHRISE_PROJECT_ID=observatorio
OECD_API_KEY=<key>
```

## Documentación del producto

- **[docs/Producto.md](docs/Producto.md)** — Documento completo del producto: visión, ICP, propuesta de valor, fuentes de datos, modelos ML, competencia, arquitectura, roadmap y métricas. Se actualiza cuando una investigación impacta esta vertical.
- **[docs/Investigaciones.md](docs/Investigaciones.md)** — Log cronológico de investigaciones procesadas que impactan esta vertical: si fortalecen o debilitan la tesis, y qué se actualizó en Producto.md como consecuencia.
- **[docs/ROADMAP.md](docs/ROADMAP.md)** — Milestones técnicos de implementación.

## Contrato AshRise

Al iniciar sesión: leer AGENTS.md → docs/ROADMAP.md → GET /state/observatorio → GET /handoffs/observatorio?status=open
Al cerrar: emitir ashrise-close con run + state_update.

## ROI estimado

| Impacto | Estimación |
|---------|-----------|
| Evitar +1% desempleo por mala policy | ~$50M/año |
| Reskilling 20% más eficiente | ~$5-10M/año |
| Exportar modelo a LATAM | Revenue adicional |
| Posicionamiento Uruguay hub IA+labor | Intangible alto |
