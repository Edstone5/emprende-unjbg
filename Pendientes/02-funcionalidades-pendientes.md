# Funcionalidades pendientes

Estas sí son decisiones técnicas mías (de alcance, no de permisos), documentadas para que el
equipo sepa exactamente qué falta y por dónde seguir. Cada punto indica en qué archivo
empezar.

## Gestión de publicaciones (afecta web y móvil por igual)

- [ ] **Editar un producto o servicio existente.** Hoy solo existe "crear"
      ([`apps/web/.../productos/nuevo/page.tsx`](../apps/web/src/app/(dashboard)/mi-emprendimiento/productos/nuevo/page.tsx),
      [`apps/mobile/app/negocio/productos/nuevo.tsx`](../apps/mobile/app/negocio/productos/nuevo.tsx)).
      No hay forma de cambiar precio, descripción o fotos después de publicar.
- [ ] **Eliminar una publicación** o marcarla como no disponible desde la UI. El campo
      `disponible` existe en la base de datos y hay una query lista
      (`alternarDisponibilidadProducto` en
      [`packages/db/src/queries/emprendimiento.ts`](../packages/db/src/queries/emprendimiento.ts)),
      pero ningún botón la llama todavía.
- [ ] **Filtro "Productos / Servicios" en el catálogo.** El esquema y la query ya lo soportan
      (`FiltroCatalogo.tipo` en
      [`packages/core/src/schemas/producto.ts`](../packages/core/src/schemas/producto.ts),
      usado en `listarProductosCatalogo`), pero no agregué el chip visual en
      `apps/web/src/app/catalogo/page.tsx` ni en `apps/mobile/app/(tabs)/buscar.tsx` — quedó
      a medio camino, solo con la insignia "Servicio" en las tarjetas.

## Perfil del emprendimiento

- [ ] **Subir el logo del emprendimiento.** La columna `logo_url` existe en la base de datos
      y el bucket `logos` ya tiene sus políticas de Storage
      ([`0003_storage.sql`](../supabase/migrations/0003_storage.sql)), pero ningún formulario
      (ni web ni móvil) tiene un campo para subirlo. `EmprendimientoForm.tsx` solo pide
      nombre, descripción, categoría y WhatsApp.
- [ ] **Editar el emprendimiento** después de creado (cambiar descripción, categoría,
      WhatsApp). Tampoco existe hoy.

## Panel de administración

- [ ] El panel (`/admin`) solo aprueba/rechaza pagos. La sección 2.8 del documento base habla
      de **moderación de contenido** (dar de baja publicaciones o perfiles que incumplan las
      normas) — no hay ninguna UI para eso; un admin tendría que editar filas directamente en
      el dashboard de Supabase.
- [ ] **Vencimiento automático de la suscripción.** Cuando un admin aprueba un pago, el
      emprendimiento pasa a `estado = 'activo'` sin fecha de expiración forzada (ver
      `revisarPago` en [`packages/db/src/queries/pago.ts`](../packages/db/src/queries/pago.ts) —
      calcula `finPeriodo` pero nunca lo guarda ni hay ningún job que lo revise). Hace falta
      una función programada (Supabase Edge Function + `pg_cron`, o un cron job de Vercel) que
      pase los emprendimientos vencidos a `estado = 'vencido'`.

## Notificaciones

- [ ] No hay notificaciones push ni por correo cuando: se aprueba/rechaza un pago, alguien
      publica un producto nuevo, o un admin modera contenido. El usuario solo se entera si
      vuelve a abrir la app.

## Web

- [ ] No hay página `error.tsx` ni `not-found.tsx` personalizada para el App Router (más allá
      del `_not-found` que genera Next por defecto) — un error real en producción mostraría la
      pantalla genérica de Next.js.
- [ ] No hay *loading skeletons* (`loading.tsx`) en las rutas que hacen fetch a Supabase — la
      recomendación de UX que di en la primera respuesta de esta conversación (esqueletos de
      carga) no llegó a implementarse.
- [ ] El modo oscuro está a medias: `globals.css` define variables CSS para
      `prefers-color-scheme: dark`, pero casi todos los componentes usan clases fijas como
      `bg-white` / `text-neutral-900` que no reaccionan a esas variables. Hoy la app es
      efectivamente solo modo claro.

## Móvil

- [ ] Mismo punto de modo oscuro: NativeWind soporta el prefijo `dark:`, pero no se usó en
      ningún componente.
- [ ] *Pull-to-refresh* solo existe en Inicio y Vender; falta en Buscar y en el detalle de
      producto. Tampoco hay caché offline del catálogo (sin conexión, las pantallas
      simplemente no cargan nada).
