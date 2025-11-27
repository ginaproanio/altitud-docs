# 02_FLUJOS_USUARIO.md
**Proyecto:** ALTITUD Catering – https://altitud.group  
**Estado:** APROBADO y CONGELADO — 26 noviembre 2025  
**Versión:** 1.0

## 1. Roles de usuario
- Cliente anónimo  
- Cliente registrado  
- Administrador (tú y tu equipo)

## 2. Flujos de Cliente (Web + App)

### Flujo 1 – Compra rápida (60 % de los pedidos)
1. Entra a altitud.group  
2. Ve productos destacados o va directo a /menu  
3. Toca “Agregar” en empanadas, humitas, etc.  
4. Mini-carrito flotante se actualiza  
5. Toca “Ir al carrito” → /carrito  
6. Revisa resumen → “Pagar con Payphone”  
7. CheckoutPayphone.vue → redirección a Payphone  
8. Paga → vuelve → webhook confirma → pedido creado + email/SMS  
9. Limpieza automática del carrito

### Flujo 2 – Reserva de catering corporativo o evento privado
1. Va a /reservas o botón grande en home  
2. Llena ReservationForm.vue (fecha, hora, personas, tipo de evento, presupuesto aprox.)  
3. Envía → llega a admin + email automático al cliente  
4. Admin responde por WhatsApp/email → convierte en pedido formal

### Flujo 3 – Seguimiento de pedido
1. Después de pagar → pantalla “¡Gracias!” con número de pedido  
2. Opción “Ver mis pedidos” (si está logueado) o por link mágico en email  
3. Ve estado en tiempo real: Preparando → En camino (Rappi) → Entregado

### Flujo 4 – Repetir pedido anterior
1. Logueado → Mis pedidos → botón “Repetir”  
2. Carrito se llena automáticamente → directo a pagar

## 3. Flujos de Administrador (panel en /admin)

Ruta protegida: https://altitud.group/admin  
(credenciales en .env → solo tú y quienes autorices)

### Flujo 5 – Dashboard diario
- Pedidos del día (pendientes, en preparación, entregados)  
- Ingresos del día (Payphone + transferencias)  
- Reservas pendientes de contactar  
- Stock crítico (ej. solo quedan 20 humitas)

### Flujo 6 – Configuración técnica (NUEVO – lo que acabas de pedir)
Sección: Configuración → Pestañas

**Pestaña META & Analytics**
- Meta Pixel ID  
- Google Analytics 4 ID  
- TikTok Pixel (opcional)  
→ Guardar → se inyecta automáticamente en <head> de Nuxt

**Pestaña Payphone**
- Modo: Sandbox / Producción  
- Client ID  
- Secret Key  
- Webhook URL automática: https://api.altitud.group/api/v1/webhooks/payphone  
→ Botón “Probar conexión”

**Pestaña Cuentas bancarias**
- Banco Pichincha – Cuenta corriente – 123456789  
- Produbanco – Ahorros – 987654321  
→ Se muestran automáticamente en email de confirmación cuando el cliente elija “Transferencia” (futuro)

**Pestaña Delivery**
- Rappi API Key  
- Webhook URL automática  
- Activar/desactivar Rappi

### Flujo 7 – Gestión manual de pedidos
- Marcar como “Pagado por transferencia”  
- Cambiar estado manualmente (útil si falla webhook)  
- Enviar email/SMS manual al cliente

### Flujo 8 – Reportes y exportar
- Exportar CSV: pedidos del mes, ingresos, productos más vendidos  
- Gráficos simples (ingresos diarios, canal de venta: web vs reservas)

## 4. Reglas visuales obligatorias (para que todo se vea ALTITUD)
- Botones principales: fondo #CFA56A (Gold), texto blanco, Inter Semibold  
- Estados de pedido:  
  Pendiente → naranja  
  Pagado → verde Leaf Green  
  En preparación → azul suave  
  Entregado → Cotopaxi White con check verde

## Fin del documento

Este flujo está 100 % congelado.  
Cualquier pantalla o comportamiento que no esté aquí → NO se desarrolla hasta actualizar este archivo.

¡Listo! Ya tienes los 3 documentos base perfectos y blindados:
00_CONVENCIONES  
01_ARQUITECTURA  
02_FLUJOS_USUARIO (con Meta, Payphone config y cuentas bancarias incluidas)