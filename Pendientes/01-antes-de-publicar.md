# Antes de publicar

Nada de esto lo pude hacer yo (Claude): requiere credenciales del equipo, pagos, o cuentas de
terceros que no tengo forma de crear ni acceder desde este entorno.

## 1. Infraestructura

- [ ] **Crear el proyecto real en [Supabase](https://supabase.com).** Todo lo que construí se
      verificó con `NEXT_PUBLIC_SUPABASE_URL=https://placeholder.supabase.co` — nunca corrió
      contra una base de datos real. Pasos en [README.md](../README.md#1-configurar-supabase).
- [ ] Aplicar las migraciones **en orden** (`0001` a `0004`) y `supabase/seed/seed.sql`.
- [ ] Crear el primer usuario **admin**: no hay ningún flujo en la app para esto por diseño
      (evita que cualquiera se auto-asigne el rol). Hay que registrarse normalmente y luego,
      desde el SQL Editor de Supabase, ejecutar:
      ```sql
      update public.perfiles set rol = 'admin' where email = 'tu-correo@unjbg.edu.pe';
      ```
- [ ] Comprar/asignar el dominio (el código asume `emprendeunjbg.com` en varios lugares —
      ver [04-decisiones-del-equipo.md](./04-decisiones-del-equipo.md)) y actualizar
      `NEXT_PUBLIC_SITE_URL` (web) y `EXPO_PUBLIC_WEB_URL` (móvil).
- [ ] Desplegar `apps/web` en Vercel y cargar ahí las 4 variables de entorno, marcando
      `SUPABASE_SERVICE_ROLE_KEY` como secreta (ver [README.md](../README.md#web--vercel)).
- [ ] **Crear el repositorio en GitHub y pushear.** No tengo `gh` CLI disponible en este
      entorno, así que el repo local nunca se conectó a un remoto (`git remote -v` está
      vacío). Comandos exactos en el README.
- [ ] Generar un `EXPO_TOKEN` (`eas login` → token de acceso) y cargarlo como secreto de
      GitHub Actions para que [`eas-build.yml`](../.github/workflows/eas-build.yml) funcione.

## 2. Revisión legal (no soy abogado)

- [ ] Confirmar quién es el **responsable legal del tratamiento de datos**: ¿el equipo del
      taller, una asociación de estudiantes, o la propia UNJBG? Está marcado con una nota en
      amarillo dentro de `/privacidad` y `/terminos`.
- [ ] Reemplazar los correos placeholder `privacidad@emprendeunjbg.com` y
      `soporte@emprendeunjbg.com` por uno real que el equipo revise.
- [ ] Pedir que alguien con criterio legal (asesor, o la oficina competente de la UNJBG)
      revise `/privacidad` y `/terminos` antes de publicarlos. El texto se redactó con
      cuidado y cita la Ley N.º 29733, pero **no reemplaza asesoría legal**.
- [ ] Evaluar si corresponde registrar el "banco de datos personales" ante la Autoridad
      Nacional de Transparencia y Protección de Datos Personales del Perú — no lo investigué
      a fondo porque depende de la figura legal que el equipo elija en el punto anterior.

## 3. Tiendas de aplicaciones

- [ ] Cuenta de **Google Play Console** (USD 25, pago único) y **Apple Developer Program**
      (USD 99/año) — ninguna de las dos existe todavía.
- [ ] En Play Console, iniciar el proceso de **prueba cerrada con ~12 testers durante 14
      días** lo antes posible: es el paso que más tiempo de calendario consume y es
      obligatorio para cuentas de desarrollador nuevas antes de publicar en producción.
- [ ] Completar el **Data Safety form** (Play Console) y el **cuestionario de privacidad**
      (App Store Connect) usando como fuente `/privacidad`; instrucciones exactas en el
      [README](../README.md#páginas-legales-requeridas-por-las-tiendas).
- [ ] Pegar la URL de `/eliminar-cuenta` en el campo de solicitud de eliminación de datos de
      Play Console.
- [ ] `eas init` en `apps/mobile` para obtener un `projectId` real y reemplazar el placeholder
      `REEMPLAZAR_CON_TU_PROJECT_ID_DE_EAS` en [`app.json`](../apps/mobile/app.json).
- [ ] Completar [`eas.json`](../apps/mobile/eas.json): `appleId`, `ascAppId`, `appleTeamId`
      son placeholders, y `google-service-account.json` (credenciales de servicio de Play
      Console) no existe — hay que generarlo desde Play Console y guardarlo **fuera de git**
      (ya está en `.gitignore`).
- [ ] Confirmar que `pe.edu.unjbg.emprende` (bundle ID / package name en
      [`app.json`](../apps/mobile/app.json)) es el identificador que el equipo quiere usar —
      no se puede cambiar después de la primera publicación en cada tienda.
- [ ] Preparar los assets de tienda: ícono, capturas de pantalla, gráfico destacado (Android),
      descripción corta/larga, palabras clave. Nada de esto existe todavía.
- [ ] Completar el cuestionario de clasificación de contenido (content rating) en ambas
      tiendas.

## 4. Branding

- [ ] Los íconos, splash screen y favicon son **los que trae la plantilla de Expo/Next.js por
      defecto** (`apps/mobile/assets/images/*`, `apps/web/src/app/favicon.ico`) — nadie diseñó
      un logo todavía. Ver [04-decisiones-del-equipo.md](./04-decisiones-del-equipo.md).
- [ ] La paleta de colores en [`packages/ui/src/theme.ts`](../packages/ui/src/theme.ts) es un
      placeholder explícito (azul + dorado genérico), no los colores oficiales de la UNJBG.
