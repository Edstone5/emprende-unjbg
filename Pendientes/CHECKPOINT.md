# Checkpoint

> Se actualiza al final de cada respuesta de Claude en este proyecto. Sirve para retomar el
> trabajo después de compactar o cerrar la conversación: pega el bloque **"Prompt para
> continuar"** de abajo en una conversación nueva de Claude Code, en este mismo directorio.

**Última actualización:** 2026-09-28 · **Último commit:** `cc50136` — docs: actualiza
CHECKPOINT.md con el rediseño visual estilo PedidosYa (pusheado a `origin/main`)

## Nota de la última respuesta — rediseño visual estilo PedidosYa

El usuario pidió que la interfaz web se pareciera a PedidosYa pero con otra paleta. Se cambió
la paleta institucional (antes azul+dorado placeholder) por **violeta (`primary`) + coral
(`accent`)** en [`packages/ui/src/theme.ts`](./../packages/ui/src/theme.ts), reflejado en las
variables CSS de [`apps/web/src/app/globals.css`](../apps/web/src/app/globals.css) — cambiar la
paleta de nuevo solo requiere tocar esos dos archivos, ningún componente tiene colores
hardcodeados fuera de la escala `primary-*`/`accent-*`.

Componentes rediseñados al patrón visual de PedidosYa:
- **[`CategoryChips.tsx`](../apps/web/src/components/CategoryChips.tsx)**: de chips de texto a
  íconos circulares con etiqueta debajo (usa el campo `icono` de
  [`categorias.ts`](../packages/core/src/constants/categorias.ts), que ya existía pero no se
  renderizaba).
- **[`ProductCard.tsx`](../apps/web/src/components/ProductCard.tsx)**: esquinas más grandes
  (`rounded-2xl`), sombra al hover, precio en píldora coral.
- **[`Navbar.tsx`](../apps/web/src/components/Navbar.tsx)**: franja superior con degradado
  violeta→coral, logo en badge circular.
- **[`SearchBar.tsx`](../apps/web/src/components/SearchBar.tsx)**: píldora con sombra (antes
  solo borde).
- Landing (`page.tsx`), catálogo (`catalogo/page.tsx`) y detalle de producto
  (`catalogo/[productoId]/page.tsx`): mismos ajustes de esquina/sombra para consistencia.

De paso se corrigió un bug preexistente (no introducido en esta respuesta): el botón CTA del
hero usaba `text-primary-950`, un tono que no existe en la escala (llega hasta 900), así que
quedaba sin color de texto aplicado.

Se creó **[`.claude/launch.json`](../.claude/launch.json)** para poder previsualizar
`pnpm dev:web` desde el navegador de Claude Code sin reiniciar el server a mano cada vez.

Verificado: `tsc --noEmit`, `pnpm --filter web lint` y revisión visual en navegador (home y
`/catalogo`, en modo claro) — el patrón de categorías/tarjetas ya se ve igual al de una app de
delivery, con la paleta violeta+coral en vez de roja.

**Hallazgo colateral, no corregido (ya estaba documentado como hueco conocido, ver
[02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md)):** el modo oscuro sigue
a medias — casi todos los componentes usan `text-neutral-900` fijo, que en modo oscuro del
sistema queda invisible sobre el fondo oscuro. Con la nueva paleta el fondo oscuro es más
saturado, así que el problema se nota más si alguien prueba con el SO en modo oscuro. Arreglarlo
de verdad implica revisar cada componente, no es parte de este cambio.

**Pusheado:** el usuario pidió `git push` después de esta respuesta — `origin/main` ya está
sincronizado hasta `cc50136`, no quedan commits locales pendientes de subir.

## Nota de la respuesta anterior (servidor + dominio)

Se relevantó `pnpm dev:web` (había quedado colgado entre sesiones) y se confirmó que fuera de
esta conversación ya se resolvió el bloqueante #1 de
[05-guia-pruebas-locales.md](./05-guia-pruebas-locales.md): existe remoto de GitHub
(`https://github.com/Edstone5/emprende-unjbg`). Sobre el dominio: se recomendó comprar solo
`emprendeunjbg.com` (coincide con lo asumido en el código, ver
[04-decisiones-del-equipo.md](./04-decisiones-del-equipo.md)) y **no** los addons de Namecheap
(SSL/hosting/VPS/DNS no aplican con Vercel).

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
