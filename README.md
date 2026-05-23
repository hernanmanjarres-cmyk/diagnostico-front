# Diagnóstico CX · Emergencias Eléctricas — Bia Energy

Herramienta interna para que los analistas de CX diagnostiquen emergencias eléctricas en tiempo real usando datos de telemedida DLMS.

## Archivos

| Archivo | Descripción |
|---|---|
| `bia_cx_emergencias.html` | Aplicación web — abrir directamente en el navegador o servir como HTML estático |
| `workflows/WF-H_Emergencias_CX.json` | Workflow n8n — importar en la instancia `aibia.app.n8n.cloud` |

## Flujo de uso

1. **Buscar medidor** — ingresa contract_id, número de medidor o código SIC
2. **Confirmar tipo de medida** — mono/bi/trifásico o indirecta + segmento del cliente (A/B/C)
3. **Ingresar variables actuales** — voltajes, corrientes y FP reportados desde telemedida
4. **Diagnóstico automático** — semáforo de variables, gráficas históricas 72h con anomalías marcadas, acciones recomendadas y mensaje listo para enviar al cliente

## Configuración

En `bia_cx_emergencias.html`, ajustar al inicio del `<script>`:

```js
const WEBHOOK_URL = 'https://aibia.app.n8n.cloud/webhook/bia-emergencias';
const BIA_TOKEN   = 'TU_TOKEN_AQUI';
```

En `workflows/WF-H_Emergencias_CX.json`, reemplazar `PEGA_AQUI_TU_METABASE_API_KEY` con la API key de Metabase.

## Escenarios de diagnóstico

| # | Condición | Severidad |
|---|---|---|
| 1 | Fase perdida (voltaje = 0 con historial > 0) | Crítico |
| 2 | Voltaje bajo (< 80 % del promedio 30d) | Atención |
| 3 | Voltaje OK + corriente = 0 (sin consumo) | Atención |
| 4 | Sobrecarga (corriente > 130 % del máximo histórico) | Atención |
| 5 | Variables normales | OK |
