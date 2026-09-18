# Checkpoint

> Se actualiza al final de cada respuesta de Claude en este proyecto. Sirve para retomar el
> trabajo después de compactar o cerrar la conversación: pega el bloque **"Prompt para
> continuar"** de abajo en una conversación nueva de Claude Code, en este mismo directorio.

**Última actualización:** 2026-09-18 · **Último commit:** `84e3f3a` — feat: paridad móvil
(fotos, suscripción) y soporte de servicios

## Estado actual

- Monorepo funcional: `pnpm install`, `pnpm lint`, `pnpm typecheck` y
  `pnpm --filter web build` pasan limpio. Nunca se corrió contra un proyecto Supabase real
  (ver [03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md)).
- Web (Next.js 16): catálogo, auth, panel de emprendedor (crear negocio, publicar
  producto/servicio, reportar pago Yape), panel admin (aprobar/rechazar pagos), `/mi-cuenta`
  con eliminación de cuenta, `/privacidad`, `/terminos`, `/eliminar-cuenta`.
- Móvil (Expo/expo-router): mismas funcionalidades salvo panel admin y edición de
  publicaciones (ver huecos exactos en
  [02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md)).
- Base de datos: 4 migraciones (`0001`–`0004`) con RLS por rol, más `productos.tipo` para
  distinguir productos de servicios.
- Repo git local con 3 commits, **sin remoto configurado** (nadie lo pusheó a GitHub todavía).

## Qué falta (resumen — ver el detalle en esta misma carpeta)

1. Todo lo de [01-antes-de-publicar.md](./01-antes-de-publicar.md): crear el proyecto
   Supabase real, pushear a GitHub, cuentas de tiendas, revisión legal.
2. Huecos de producto listados en
   [02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md) (editar/eliminar
   publicaciones, logo del emprendimiento, moderación, vencimiento automático de suscripción).
3. Nada tiene pruebas automatizadas (ver
   [03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md)).

## Prompt para continuar

```
Retomo el proyecto Emprende UNJBG (catálogo de emprendimientos UNJBG, monorepo
Next.js 16 + Expo + Supabase) en C:\UNJBG\PREGRADO\ESIS\8\TALLER DE EMPRENDIMIENTO\EMPRENDE
EMPRENDEDOR. Lee Pendientes/CHECKPOINT.md y el resto de la carpeta Pendientes/ para
recuperar el estado exacto antes de seguir. Sigue actualizando Pendientes/CHECKPOINT.md al
final de cada respuesta, como veníamos haciendo.

Lo siguiente que quiero que hagas: <describe aquí qué sigue>
```
