# 04_API_CONTRACT.md
**Proyecto:** ALTITUD Catering – https://api.altitud.group  
**Estado:** APROBADO y CONGELADO — 26 noviembre 2025  
**Versión:** 1.0 — Contrato inamovible

## Base URL
https://api.altitud.group/api/v1

## Headers obligatorios en TODOS los requests
Authorization: Bearer <jwt_access_token>
Content-Type: application/json
Idempotency-Key: <uuid-v4> → obligatorio en POST/PATCH que modifican datos

## Auth
POST /auth/login
→ { "email": "juan@altitud.group", "password": "..." }
← { "access_token": "...", "refresh_token": "...", "expires_in": 3600 }

POST /auth/refresh
→ { "refresh_token": "..." }
← nuevo access_token

## Productos (públicos)
GET /products
← Lista completa activa
[
  {
    "id": "uuid",
    "nombre": "Empanada de viento",
    "precio": 2.50,
    "categoria": "empanadas",
    "imagen_url": "https://altitud.group/img/empanada.jpg"
  }
]

## Carrito y Pedidos
POST /orders/create
→ {
    "items": [
      { "producto_id": "uuid", "cantidad": 6 },
      { "producto_id": "uuid", "cantidad": 12 }
    ],
    "direccion_entrega": "Av. Amazonas y Naciones Unidas",
    "notas": "Timbre roto, tocar 0999..."
  }
← { "pedido_id": "uuid", "numero_pedido": "ALT-2025-0123", "total": 78.00 }

GET /orders/{pedido_id}
← Estado en tiempo real + items

GET /orders/my-orders  → solo usuario logueado
← Lista de sus pedidos con estado

## Webhook Payphone (crítico)
POST /webhooks/payphone
→ Payload completo de Payphone + headers
← HTTP 200 en < 3 segundos
Acciones automáticas:
- Si transactionStatus = 1 (Approved) → estado = "pagado"
- Si ya existe idempotency_key → 200 sin hacer nada
- Guarda todo en webhooks_log

## Webhook Rappi (crítico)
POST /webhooks/rappi
→ Payload Rappi
← HTTP 200 inmediato
- Actualiza estado: "en_camino", "entregado", etc.
- Idempotency-Key obligatorio

## Reservas Catering
POST /reservations
→ {
    "nombre_contacto": "María López",
    "telefono": "0999123456",
    "email": "maria@empresa.com",
    "fecha_evento": "2025-12-20",
    "personas": 80,
    "tipo_evento": "corporativo",
    "presupuesto_aprox": 2500.00,
    "mensaje": "Coffee break + almuerzo"
  }
← { "reserva_id": "uuid", "mensaje": "¡Recibido! Te contactamos en < 2h" }

GET /reservations → solo admin
← Lista completa con filtros

PATCH /reservations/{id} → solo admin
→ { "estado": "contactado" } o "convertido" + pedido_id

## Panel Admin (protegido)
GET /admin/dashboard
← {
    "hoy": { "pedidos": 24, "ingresos": 890.50 },
    "pendientes": 5,
    "reservas_pendientes": 3
  }

GET /admin/config
← todas las filas de configuracion_sistema

PATCH /admin/config
→ { "meta_pixel_id": "123456789", "payphone_sandbox": false, ... }

## Errores estándar (siempre así)
{
  "error": "invalid_idempotency_key",
  "message": "El Idempotency-Key es obligatorio",
  "status": 400
}

Códigos comunes:
400 → datos malos
401 → token expirado
409 → idempotency_key duplicado (ya procesado)
500 → nunca debe pasar

## Fin del contrato

Este API contract está 100 % congelado.
Ningún endpoint nuevo se agrega sin actualizar este archivo y aprobarlo contigo primero.
¡NUNCA MÁS UN PARCHE!

Copia-pega y ya tienes los 5 documentos sagrados:
00_CONVENCIONES  
01_ARQUITECTURA  
02_FLUJOS_USUARIO  
03_MODELADO_BD  
04_API_CONTRACT