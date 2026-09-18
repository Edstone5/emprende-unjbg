# Checkpoint

> Se actualiza al final de cada respuesta de Claude en este proyecto. Sirve para retomar el
> trabajo después de compactar o cerrar la conversación: pega el bloque **"Prompt para
> continuar"** de abajo en una conversación nueva de Claude Code, en este mismo directorio.

**Última actualización:** 2026-09-18 · **Último commit:** `e99b68a` — docs: actualiza
CHECKPOINT.md con el último commit (sin commits de código nuevos desde entonces — ver nota
abajo)

## Nota de la última respuesta (no es un commit, es config local + estado del entorno)

Se volvió a levantar `pnpm dev:web` en background (la sesión anterior había quedado colgada, el
proceso ya no corría) — confirmado arriba en `http://localhost:3000`, cargando `.env.local`
correctamente.

Se detectó que **fuera de esta conversación ya se resolvió el bloqueante #1**: ahora existe
remoto de GitHub (`https://github.com/Edstone5/emprende-unjbg`) y está sincronizado hasta
`e99b68a` — cualquier colaborador ya puede clonar (paso 1 de
[05-guia-pruebas-locales.md](./05-guia-pruebas-locales.md) queda resuelto).

El usuario preguntó si es urgente elegir alguno de los addons que Namecheap ofrece junto al
dominio (`emprendeunjbg.com`, ya en el carrito, coincide con lo asumido en el código — ver
[04-decisiones-del-equipo.md](./04-decisiones-del-equipo.md)). Respuesta: no, solo el dominio
hace falta. SSL/hosting/VPS/DNS premium no aplican porque el deploy es en Vercel (HTTPS
automático); Business Email podría servir más adelante para `privacidad@`/`soporte@` pero
depende de una decisión pendiente (responsable legal) — no es urgente ahora.

**Sigue pendiente de confirmar (no verificado aún):** si las migraciones (`0001`–`0004`) y el
seed ya se aplicaron en el proyecto Supabase real (`hvjfwncpjtsxpzodvyaa.supabase.co`). Si el
catálogo o el registro fallan al probar la web, probablemente sea por eso.

## Estado actual

- Monorepo funcional: `pnpm install`, `pnpm lint`, `pnpm typecheck` y
  `pnpm --filter web build` pasan limpio. Nunca se corrió contra un proyecto Supabase real
  (ver [03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md)) — sigue siendo así, todavía nadie
  lo probó contra datos reales.
- Web (Next.js 16): catálogo, auth, panel de emprendedor (crear negocio, publicar
  producto/servicio, reportar pago Yape), panel admin (aprobar/rechazar pagos), `/mi-cuenta`
  con eliminación de cuenta, `/privacidad`, `/terminos`, `/eliminar-cuenta`.
- Móvil (Expo/expo-router): mismas funcionalidades salvo panel admin y edición de
  publicaciones (ver huecos exactos en
  [02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md)).
- Base de datos: 4 migraciones (`0001`–`0004`) con RLS por rol, más `productos.tipo` para
  distinguir productos de servicios.
- Repo git local con 5 commits en `main`, **sin remoto configurado todavía** (`git remote -v`
  vacío; este entorno no tiene `gh` CLI instalado). Es el bloqueante #1 para que el equipo
  pueda clonar — ver la guía nueva de abajo.

## Qué se hizo en esta respuesta

El usuario preguntó si ya se puede probar la web en local, pidió los pasos para que cualquier
colaborador la clone y la corra, y un recordatorio de qué falta para desplegarla. Se creó
**[05-guia-pruebas-locales.md](./05-guia-pruebas-locales.md)**: pasos exactos para (1) crear el
repo en GitHub y pushear (no existe remoto — quien tenga la cuenta del equipo debe correr
`gh repo create` o crearlo manual y `git remote add origin ...` + `git push -u origin main`,
esto no lo pude hacer yo, no tengo `gh` CLI ni credenciales en este entorno), (2) que cada
colaborador clone e instale (`git clone`, Node 20+, `corepack enable`), (3) crear **un solo
proyecto Supabase compartido** (`emprende-unjbg-dev`, distinto del que se cree luego para
producción) para que el equipo pruebe el mismo flujo entre varias cuentas, y (4) un checklist
de prueba manual de punta a punta. Confirmé que `npx supabase` (CLI) funciona en este entorno
sin instalación previa (`2.117.0`), útil para `db push` en vez de pegar SQL a mano.

## Qué falta (resumen — ver el detalle en esta misma carpeta)

1. Todo lo de [01-antes-de-publicar.md](./01-antes-de-publicar.md): crear el proyecto
   Supabase real, pushear a GitHub, cuentas de tiendas, revisión legal, dominio.
2. Huecos de producto listados en
   [02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md) (editar/eliminar
   publicaciones, logo del emprendimiento, moderación, vencimiento automático de suscripción).
3. Nada tiene pruebas automatizadas (ver
   [03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md)).
4. Ejecutar de verdad el checklist de
   [05-guia-pruebas-locales.md](./05-guia-pruebas-locales.md) contra un proyecto Supabase real
   — sigue sin hacerse.

## Prompt para continuar

```
Retomo el proyecto Emprende UNJBG (catálogo de emprendimientos UNJBG, monorepo
Next.js 16 + Expo + Supabase) en C:\UNJBG\PREGRADO\ESIS\8\TALLER DE EMPRENDIMIENTO\EMPRENDE
EMPRENDEDOR. Lee Pendientes/CHECKPOINT.md y el resto de la carpeta Pendientes/ para
recuperar el estado exacto antes de seguir. Sigue actualizando Pendientes/CHECKPOINT.md al
final de cada respuesta, como veníamos haciendo.

Lo siguiente que quiero que hagas: <describe aquí qué sigue>
```
