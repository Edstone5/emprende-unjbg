# Checkpoint

> Se actualiza al final de cada respuesta de Claude en este proyecto. Sirve para retomar el
> trabajo después de compactar o cerrar la conversación: pega el bloque **"Prompt para
> continuar"** de abajo en una conversación nueva de Claude Code, en este mismo directorio.

**Última actualización:** 2026-09-28 · **Último commit:** `9c4fd0b` — docs: actualiza la guía
de pruebas locales y CHECKPOINT.md (repo ya en GitHub)

## Nota de la última respuesta — la guía de pruebas locales ya existía, se actualizó

El usuario preguntó si ya había una guía para probar la web en local. Sí:
**[05-guia-pruebas-locales.md](./05-guia-pruebas-locales.md)**, creada dos respuestas atrás.
Estaba desactualizada en un punto — decía "no existe repositorio remoto todavía", pero eso se
resolvió hace dos respuestas. Se corrigió esa sección y la de "Estado actual" más abajo en este
mismo archivo (tenían la misma información vieja). Confirmado en esta respuesta, en vivo:
`origin/main` sincronizado, `.env.local` existe, `pnpm dev:web` corriendo en
`http://localhost:3000`. Lo único que sigue sin confirmar es si el proyecto Supabase real ya
tiene las migraciones/seed aplicados (paso 3 de la guía) — no se verificó, sigue igual desde
hace varias respuestas.

## Nota de la respuesta anterior — rediseño visual estilo PedidosYa

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

## Estado actual

- Monorepo funcional: `pnpm install`, `pnpm lint`, `pnpm typecheck` y
  `pnpm --filter web build` pasan limpio.
- Esta máquina ya tiene `apps/web/.env.local` apuntando a un proyecto Supabase **real**
  (`hvjfwncpjtsxpzodvyaa.supabase.co`) y `pnpm dev:web` corriendo en
  [http://localhost:3000](http://localhost:3000) — pero **sigue sin confirmarse si las
  migraciones `0001`–`0004` y el seed ya se aplicaron ahí** (ver nota de dos respuestas atrás,
  más abajo). Nadie más del equipo lo ha probado todavía contra datos reales.
- Web (Next.js 16): catálogo, auth, panel de emprendedor (crear negocio, publicar
  producto/servicio, reportar pago Yape), panel admin (aprobar/rechazar pagos), `/mi-cuenta`
  con eliminación de cuenta, `/privacidad`, `/terminos`, `/eliminar-cuenta`. Rediseño visual
  estilo PedidosYa (violeta+coral) aplicado — ver nota de la respuesta anterior.
- Móvil (Expo/expo-router): mismas funcionalidades salvo panel admin y edición de
  publicaciones (ver huecos exactos en
  [02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md)).
- Base de datos: 4 migraciones (`0001`–`0004`) con RLS por rol, más `productos.tipo` para
  distinguir productos de servicios.
- Repo en GitHub: [`Edstone5/emprende-unjbg`](https://github.com/Edstone5/emprende-unjbg)
  (privado), sincronizado con `main` local. El bloqueante #1 de la guía de pruebas locales
  (crear el remoto) **ya está resuelto** — falta el proyecto Supabase compartido para el
  equipo (paso 3 de la guía), eso no está confirmado.

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
