# Guía: probar la web en local y dar acceso al equipo

Responde a dos preguntas: **¿ya se puede probar la web en una máquina?** y **¿cómo hace
cualquier colaborador para clonar y correrla en la suya?**

## Respuesta corta

**Sí, el código está listo para correr en local** (`pnpm lint`/`typecheck`/`build` pasan), pero
faltan dos cosas que no son de código, sino de configuración de equipo, antes de que un
colaborador pueda clonar y probar:

1. **No existe repositorio remoto todavía.** Hay 5 commits en `main` en este equipo local, pero
   `git remote -v` está vacío y este entorno no tiene `gh` CLI — nadie puede clonar algo que no
   está en ningún servidor. Hace falta crear el repo en GitHub y pushear (paso 1 abajo).
2. **No hay un proyecto Supabase compartido.** El README actual asume que cada quien crea su
   propio proyecto Supabase gratuito — funciona para probar la UI, pero si dos personas del
   equipo quieren probar el mismo flujo (uno publica un producto, otro lo aprueba como admin)
   necesitan apuntar al **mismo** proyecto. Recomendado: un solo proyecto Supabase "dev"
   compartido por todo el equipo (paso 3 abajo).

## Paso 1 — Crear el repositorio en GitHub y pushear (lo hace quien tenga la cuenta del equipo)

```bash
gh repo create emprende-unjbg --private --source=. --remote=origin
git push -u origin main
```

Si no tienes `gh` CLI: crea el repo vacío manualmente en github.com (sin README/licencia, para
no chocar con lo que ya existe local) y luego:

```bash
git remote add origin https://github.com/<tu-organizacion-o-usuario>/emprende-unjbg.git
git push -u origin main
```

Recomendado **privado** por ahora (hay decisiones sin cerrar y no es código listo para
publicar). Luego, en GitHub → Settings → Collaborators, agrega a cada miembro del equipo (o
crea un equipo/organización si van a ser varios repos).

Los placeholders en [`.github/CODEOWNERS`](../.github/CODEOWNERS) (`@equipo-ui`,
`@equipo-servidor`, `@equipo-qa`) hay que reemplazarlos por los usuarios reales de GitHub una
vez que el equipo tenga cuenta — ver [04-decisiones-del-equipo.md](./04-decisiones-del-equipo.md).

## Paso 2 — Cada colaborador clona el repo (cualquier SO: Windows/Mac/Linux)

Requisitos en la máquina de cada persona:

- **Git**
- **Node.js 20+** (el repo fija `20` en [`.nvmrc`](../.nvmrc); usar
  [nvm](https://github.com/nvm-sh/nvm) o [nvm-windows](https://github.com/coreybutler/nvm-windows)
  evita líos de versión entre máquinas distintas)
- **pnpm 9+** — con Node instalado: `corepack enable` (trae pnpm automáticamente porque
  `package.json` fija `"packageManager": "pnpm@9.15.0"`)

```bash
git clone https://github.com/<tu-organizacion-o-usuario>/emprende-unjbg.git
cd emprende-unjbg
pnpm install
```

## Paso 3 — Un proyecto Supabase compartido para el equipo (lo crea una sola persona)

1. En [supabase.com](https://supabase.com), crear un proyecto nuevo — nómbralo algo como
   `emprende-unjbg-dev` para distinguirlo claramente del que se cree más adelante para
   producción (ver [01-antes-de-publicar.md](./01-antes-de-publicar.md)).
2. Aplicar el esquema, en orden, desde el SQL Editor del dashboard: pegar y ejecutar el
   contenido de `supabase/migrations/0001_init.sql`, `0002_rls.sql`, `0003_storage.sql` y
   `0004_producto_tipo.sql`, uno por uno. (Alternativa con CLI:
   `npx supabase link --project-ref <ref>` y luego `npx supabase db push` desde la raíz del
   repo — ya verifiqué que `npx supabase` funciona sin instalación previa, versión `2.117.0`.)
3. Ejecutar `supabase/seed/seed.sql` para cargar las 10 categorías.
4. En **Authentication → Providers**, dejar habilitado email/password (viene así por defecto).
5. En **Authentication → URL Configuration**, poner `http://localhost:3000` como Site URL y
   agregar `http://localhost:3000/auth/callback` a Redirect URLs — si no, el link de
   confirmación de correo no vuelve a la app.
6. Copiar de **Project Settings → API**: `Project URL`, `anon public key` y `service_role key`.
7. Compartir esos tres valores con el equipo **por un canal privado** (gestor de contraseñas
   compartido, o el chat privado del equipo) — nunca subirlos a GitHub ni pegarlos en un canal
   público. La `service_role key` en particular salta todas las reglas de seguridad (RLS); si
   se filtra, hay que rotarla desde el mismo panel de Supabase.

Cada colaborador, en su copia local:

```bash
cp apps/web/.env.example apps/web/.env.local
```

Y completa `apps/web/.env.local` con los tres valores del paso anterior:

```
NEXT_PUBLIC_SUPABASE_URL=<Project URL>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<anon public key>
SUPABASE_SERVICE_ROLE_KEY=<service_role key>
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

`.env.local` ya está en `.gitignore` — nunca se commitea.

## Paso 4 — Correr la web

```bash
pnpm dev:web
```

Abrir [http://localhost:3000](http://localhost:3000). Cada colaborador la corre en su propia
máquina, apuntando todos al mismo proyecto Supabase compartido — así lo que uno publica lo ve
el otro.

## Paso 5 — Probar el flujo completo (checklist)

- [ ] Registrarse con un correo `@unjbg.edu.pe` (el registro rechaza otros dominios) y
      confirmar el correo (revisa spam; el remitente por defecto de Supabase Auth no está
      personalizado todavía — ver [03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md)).
- [ ] Crear un emprendimiento desde `/mi-emprendimiento`.
- [ ] Publicar un producto **y** un servicio, para confirmar que el campo `tipo` funciona de
      punta a punta (esto es justo lo que nunca se probó contra una base de datos real).
- [ ] Reportar un pago Yape de prueba desde `/mi-emprendimiento/suscripcion`.
- [ ] Volver al SQL Editor de Supabase y ejecutar
      `update public.perfiles set rol = 'admin' where email = 'tu-correo@unjbg.edu.pe';` con
      una segunda cuenta de prueba, para poder entrar a `/admin` y aprobar/rechazar ese pago.
- [ ] Confirmar que el producto/servicio aparece en `/catalogo` para un usuario no autenticado.

Si algo de esto falla, es información nueva y real (a diferencia de todo lo demás en esta
carpeta, que son huecos conocidos de antemano) — anótalo en
[03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md) o abre un issue en GitHub una vez que el
repo esté ahí.

## Nota sobre la app móvil

Mismo patrón (`cp apps/mobile/.env.example apps/mobile/.env`, mismas credenciales de Supabase,
más `EXPO_PUBLIC_WEB_URL=http://localhost:3000`) y `pnpm dev:mobile` + Expo Go. No se cubre en
detalle aquí porque el pedido de esta guía fue específicamente la web; el README tiene los
pasos básicos en la sección "4. Levantar la app móvil".
