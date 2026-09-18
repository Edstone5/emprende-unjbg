# Calidad y riesgos técnicos

## Lo que se verificó vs. lo que no

Todo lo construido pasa `pnpm lint`, `pnpm typecheck` y `pnpm --filter web build` con
variables de entorno de **relleno** (`placeholder.supabase.co`). Eso confirma que el código
compila y que los tipos encajan entre `packages/db` y las apps — **no** confirma que:

- [ ] Las políticas RLS de [`0002_rls.sql`](../supabase/migrations/0002_rls.sql) se comporten
      como se espera con usuarios reales de cada rol (consumidor, emprendedor, admin). Se
      diseñaron con cuidado y siguiendo el patrón estándar de Supabase, pero nunca se
      ejecutó una sola query contra una base de datos real.
- [ ] El flujo completo de registro → confirmación de correo → `/auth/callback` → sesión
      activa funcione de punta a punta (depende de la configuración de Auth del proyecto
      Supabase real, que no existe todavía).
- [ ] `/api/cuenta` (eliminar cuenta) borre correctamente en cascada y limpie Storage contra
      datos reales. La lógica está razonada con cuidado (ver
      [`apps/web/src/app/api/cuenta/route.ts`](../apps/web/src/app/api/cuenta/route.ts)) pero
      nunca se ejecutó contra un proyecto real.
- [ ] La subida de imágenes desde el celular (`fetch(uri).then(r => r.arrayBuffer())` en
      [`apps/mobile/components/ImageUploader.tsx`](../apps/mobile/components/ImageUploader.tsx))
      funcione igual en Android e iOS — es el patrón recomendado por la documentación de Expo,
      pero no se probó en un dispositivo físico ni en un emulador.
- [ ] Los builds de EAS (`eas build`) compilen sin errores nativos — nunca se ejecutó un build
      real (requiere una cuenta de Expo, ver
      [01-antes-de-publicar.md](./01-antes-de-publicar.md)).

**Antes de dar por buena cualquier parte de la app, pruébala manualmente contra el proyecto
real de Supabase con al menos dos cuentas (una emprendedor, una admin).**

## Sin pruebas automatizadas

No se escribió ningún test (unitario, de integración o end-to-end) para ninguna de las dos
apps. `turbo.json` tiene una tarea `test` definida, pero ningún `package.json` implementa un
script `test` real. Sugerencia de prioridad si el equipo quiere empezar:

1. Tests unitarios de los esquemas Zod en `packages/core` (son puros, sin dependencias, fáciles
   de testear con Vitest).
2. Un test de integración del flujo "crear emprendimiento → publicar producto → aparece en el
   catálogo" contra una instancia local de Supabase (`supabase start`).
3. Tests E2E con Playwright para la web (login, catálogo, publicar producto).

## Seguridad

- No se hizo una revisión de seguridad dedicada (más allá del diseño de RLS y de no exponer
  `SUPABASE_SERVICE_ROLE_KEY` al cliente).
- No hay *rate limiting* en los endpoints de autenticación ni en `/api/cuenta` — alguien podría
  intentar fuerza bruta contra el login sin que nada lo frene a nivel de aplicación (Supabase
  Auth tiene sus propios límites por defecto, pero no se revisaron ni ajustaron).
- El regex de validación de WhatsApp (`packages/core/src/schemas/*.ts`) es permisivo a
  propósito; no valida que el número realmente tenga WhatsApp activo.

## Accesibilidad

No se hizo ninguna revisión de accesibilidad (lectores de pantalla, contraste de color,
tamaños de touch target en móvil). Los componentes usan buenas prácticas básicas (`aria-hidden`
en íconos decorativos, `alt` en imágenes) pero no fue una pasada dedicada.

## Supuestos técnicos que el equipo debe validar

- **Versiones de paquetes**: durante el desarrollo encontré varias veces que mi conocimiento
  de versiones de `@supabase/ssr`, `expo-image`, `expo-image-picker`, `eslint-config-expo`,
  etc. estaba desactualizado frente a lo publicado hoy en npm (Next.js 16, Expo SDK 57 y sus
  paquetes asociados salieron después de mi fecha de corte de conocimiento). Corregí cada caso
  verificando contra el registro de npm y las docs empaquetadas en `node_modules`, pero el
  equipo debería correr `pnpm outdated` periódicamente y no asumir que las versiones fijadas
  hoy (18 de septiembre de 2026) van a seguir siendo las recomendadas dentro de unos meses.
- El límite de precio (`precio.max(50000)` en
  [`packages/core/src/schemas/producto.ts`](../packages/core/src/schemas/producto.ts)) y el
  precio de la suscripción (`PRECIO_SUSCRIPCION_MENSUAL = 10` en
  [`packages/core/src/constants/suscripcion.ts`](../packages/core/src/constants/suscripcion.ts))
  son valores razonables que elegí, no cifras que el equipo haya confirmado.
- Los templates de correo de Supabase Auth (confirmación de cuenta, etc.) son los que trae
  Supabase por defecto, en inglés — no se personalizaron con la marca del proyecto.
