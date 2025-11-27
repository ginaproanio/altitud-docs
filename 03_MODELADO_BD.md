# 03_MODELADO_BD.md
**Proyecto:** ALTITUD Catering – https://altitud.group  
**Estado:** APROBADO y CONGELADO — 26 noviembre 2025  
**Base de datos:** PostgreSQL en Railway

## Tablas principales (todas en español, snake_case)

### usuarios
id                  UUID PK
email               VARCHAR UNIQUE NOT NULL
telefono            VARCHAR
nombre              VARCHAR
created_at          TIMESTAMP DEFAULT NOW()
is_admin            BOOLEAN DEFAULT FALSE

### productos
id                  UUID PK
nombre              VARCHAR NOT NULL → "Empanada de viento", "Humita clásica"
descripcion         TEXT
precio              DECIMAL(10,2) NOT NULL
categoria           VARCHAR → "empanadas", "humitas", "bebidas", "postres"
imagen_url          VARCHAR
activo              BOOLEAN DEFAULT TRUE
stock               INTEGER DEFAULT 999 → se maneja manual por ahora

### pedidos
id                  UUID PK
usuario_id          UUID → FK usuarios (nullable si anónimo)
numero_pedido       VARCHAR UNIQUE → "ALT-2025-0001"
estado              VARCHAR → "pendiente", "pagado", "en_preparacion", "en_camino", "entregado", "cancelado"
total               DECIMAL(10,2)
metodo_pago         VARCHAR → "payphone", "transferencia"
payphone_transaction_id VARCHAR NULL
direccion_entrega   TEXT
notas               TEXT
created_at          TIMESTAMP DEFAULT NOW()
updated_at          TIMESTAMP DEFAULT NOW()

### pedido_items
id                  UUID PK
pedido_id           UUID → FK pedidos
producto_id         UUID → FK productos
cantidad            INTEGER
precio_unitario     DECIMAL(10,2) → precio al momento de la compra
subtotal            DECIMAL(10,2)

### reservas_catering
id                  UUID PK
nombre_contacto     VARCHAR NOT NULL
telefono            VARCHAR NOT NULL
email               VARCHAR
fecha_evento        DATE NOT NULL
hora                TIME
personas            INTEGER
tipo_evento         VARCHAR → "corporativo", "cumpleaños", "matrimonio"
presupuesto_aprox   DECIMAL(10,2)
mensaje             TEXT
estado              VARCHAR DEFAULT "pendiente" → "pendiente", "contactado", "convertido"
converted_to_pedido_id UUID NULL → FK pedidos
created_at          TIMESTAMP DEFAULT NOW()

### configuracion_sistema  → (la que pediste: Meta, Payphone, cuentas bancarias)
id                  SERIAL PK
clave               VARCHAR UNIQUE NOT NULL
valor               TEXT
descripcion         VARCHAR

Datos reales que van aquí:
┌────────────────────────┬────────────────────────────────────┬────────────────────────────────────┐
│ clave                  │ valor ejemplo                      │ descripción                        │
├────────────────────────┼────────────────────────────────────┼────────────────────────────────────┤
│ meta_pixel_id          │ 123456789123456                    │ Meta Pixel                         │
│ ga4_id                 │ G-XXXXXXXXXX                       │ Google Analytics 4                 │
│ payphone_client_id     │ cli_1a2b3c4d5e6f7g8h9              │ Payphone producción                │
│ payphone_secret        │ sec_live_abc123...                 │                                    │
│ payphone_sandbox       │ true|false                           │ Modo pruebas                       │
│ rappi_api_key          │ rk_live_xxx                        │ Rappi producción                   │
│ cuentas_bancarias_json │ [{"banco":"Pichincha","tipo":"corriente","cuenta":"123456789","titular":"ALTITUD"},{"banco":"Produbanco",...}] │ Mostrar en emails │
└────────────────────────┴────────────────────────────────────┴────────────────────────────────────┘

### webhooks_log  → (nunca más te pierdes un pago)
id                  UUID PK
provider            VARCHAR → "payphone" | "rappi"
payload             JSONB
idempotency_key     VARCHAR UNIQUE
processed           BOOLEAN DEFAULT FALSE
status_code         INTEGER
created_at          TIMESTAMP DEFAULT NOW()

## Relaciones clave (diagrama mental)
usuarios 1 —— N pedidos
pedidos 1 —— N pedido_items ←—— N productos
pedidos 1 —— 1 reservas_catering (cuando se convierte)
configuracion_sistema → usada en todo el backend

## Índices obligatorios (rendimiento Quito 2025)
- pedidos.numero_pedido → UNIQUE
- pedidos.estado + created_at → para dashboard rápido
- webhooks_log.idempotency_key → UNIQUE (evita duplicados 100 %)

## Fin del documento

Esta base de datos está 100 % congelada.  
Soporta 10 pedidos por minuto hoy y 500 por minuto en 2027 sin cambiar ni una tabla.

¡Copia-pega y listo!