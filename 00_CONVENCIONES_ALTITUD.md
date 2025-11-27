# **00_CONVENCIONES_ALTITUD.md**
---
# CONVENCIÓNES OFICIALES DEL PROYECTO ALTITUD CATERING (2025)
**Versión definitiva — 26 noviembre 2025 — 100 % blindada**
Este documento define estándares obligatorios para que cualquier desarrollador trabaje sin perderse, sin improvisar y sin romper la arquitectura del proyecto.
---

# 1. Principios del Proyecto
1. UX primero: Navegación simple, compra rápida, reservas claras.
2. Consistencia total: Misma paleta, misma tipografía, mismo estilo en toda la web/app.
3. Arquitectura limpia: Todo modular, nada duplicado, nada “inventado” en páginas.
4. Regla Suprema:
   > Si una convención rompe el flujo de compra, pago o reservas → se revierte.
5. Documentación obligatoria: Ningún cambio sin actualizar este archivo.

# 2. Convenciones de Nombres

## 2.1 Archivos
- Frontend (Vue/Nuxt): kebab-case → menu-page.vue, product-card.vue
- Backend (Python): snake_case → order_controller.py
- Componentes UI: PascalCase → MenuCard.vue, CheckoutPayphone.vue

## 2.2 Variables
- JS/TS: camelCase
- Python: snake_case

## 2.3 Base de Datos
- Tablas y columnas en español, snake_case → producto, pedido, cliente, proveedor_id

## 2.4 API
- Siempre en inglés → /api/v1/orders/create

## 2.5 URLs públicas (UX)
- Siempre en español → /menu, /reservas, /catering, /carrito

## 2.6 Variables de entorno (obligatorias)
# Frontend (Nuxt/Capacitor)
VITE_API_URL=https://api.altitud.group
VITE_APP_ENV=production|development

# Backend (FastAPI)
DATABASE_URL=postgresql://...
PAYPHONE_SECRET=...
RAPPI_WEBHOOK_SECRET=...
JWT_SECRET=...
JWT_REFRESH_SECRET=...
IDEMPOTENCY_EXPIRY=86400  # segundos

# 3. Diseño (UI/UX)

## 3.1 Paleta Oficial
- Altitud Gold: #CFA56A
- Andes Black: #1C1C1C
- Earth Brown: #5A3E2B
- Cotopaxi White: #FFFFFF
- Leaf Green: #7BAE74

## 3.2 Tipografías
- Titulares: Playfair Display
- Texto: Inter
- Botones: Inter Semibold

## 3.3 Componentes obligatorios (nunca duplicar)
Header.vue | Footer.vue | MenuCard.vue | ProductCard.vue | CheckoutPayphone.vue | ReservationForm.vue

# 4. Arquitectura
- No lógica dentro de componentes
- No repetir código en páginas
- No consultas directas a la API desde componentes
- Servicios centralizados (services/api.ts)
- Componentes 100 % reutilizables

# 5. Contenido
- Textos cortos y claros → “Ordenar ahora”, “Reservar”, “Ver menú”
- Fotos: 1080×1080, fondo blanco o madera rústica, nunca pixeladas

# 6. Funcionalidades clave
- Carrito → localStorage + limpieza automática tras pago
- Payphone → flujo completo con webhook + idempotency-key obligatorio
- Rappi → webhook obligatorio + idempotency-key
- Estados de pedido solo cambian por webhook (nunca por frontend)

# 7. Seguridad
- SSL obligatorio (Cloudflare Full strict)
- Sanitización de todos los inputs
- No almacenar datos de tarjeta
- Tokens JWT expiran en 1h (access) / 30d (refresh)
- .env nunca se sube al repo
- CORS solo estos orígenes:
  https://altitud.group
  https://www.altitud.group
  capacitor://localhost
  http://localhost:*

# 8. Git
- Ramas: main → producción | dev → desarrollo | feature/nombre | fix/nombre
- Commits:
  feat: add catering reservation flow
  fix: correct Payphone callback error
  style: adjust spacing in product grid

# 9. Despliegue y Dominios
- Dominio oficial: https://altitud.group y https://www.altitud.group
- DNS: Cloudflare con proxy naranja + SSL Full (strict)
- Hosting: Railway.app (dos proyectos separados)
  → altitud-web (frontend Nuxt 3 SSR)
  → altitud-api (backend FastAPI)

## Comandos exactos Railway (nunca cambian)

### altitud-web
Build Command: npm run build
Start Command: node .output/server/index.mjs
Variables: NITRO_HOST=0.0.0.0

### altitud-api
Build Command: (vacío)
Start Command: uvicorn app.main:app --host 0.0.0.0 --port $PORT --workers 2

# Fin del archivo.

Este documento está 100 % congelado.
Cualquier código que no cumpla esto será rechazado automáticamente.
¡NADA SE IMPROVISA!