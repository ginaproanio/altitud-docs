
## 3. Despliegue Railway (comandos exactos e inamovibles)

### Proyecto: altitud-web
- Build Command:   npm run build
- Start Command:   node .output/server/index.mjs
- Variables obligatorias: NITRO_HOST=0.0.0.0

### Proyecto: altitud-api
- Build Command:   (vacío)
- Start Command:   uvicorn app.main:app --host 0.0.0.0 --port $PORT --workers 2

## 4. Flujo de datos (nunca se rompe)

Usuario → https://altitud.group (Nuxt SSR en Railway)  
→ Llamadas REST a https://api.altitud.group/api/v1/...  
→ FastAPI → PostgreSQL  
→ Webhooks Payphone/Rappi → actualizan pedido en BD (idempotency-key garantiza no duplicar)

## 5. Reglas de oro (ley absoluta)

1. Nunca lógica de negocio en componentes Vue  
2. Nunca llamada directa a API desde páginas o componentes → siempre services/api.ts  
3. Nunca duplicar componentes → si se usa en 2 páginas, va a components/  
4. Todo endpoint que modifica datos requiere idempotency-key  
5. Nunca guardar datos de tarjeta → Payphone lo hace todo  
6. El carrito solo se limpia con webhook de pago confirmado

## Fin del documento

Este archivo está 100 % congelado.
Cualquier código o arquitectura que se salga de esto será rechazado automáticamente.
¡NADA SE IMPROVISA!

Listo. Copia-pega de una sola vez y ya tienes la arquitectura más limpia y profesional de todo Ecuador en 2025.