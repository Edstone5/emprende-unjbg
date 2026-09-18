# Pendientes

Esta carpeta es la lista honesta de todo lo que falta, lo que no se pudo hacer, y por qué,
generada mientras Claude construía el monorepo. No es un backlog de ideas nuevas: es el
registro de huecos reales entre lo que existe hoy en el código y lo que hace falta para que
el proyecto esté listo para producción y para las tiendas.

Cuatro documentos, de más a menos urgente:

1. **[01-antes-de-publicar.md](./01-antes-de-publicar.md)** — bloqueantes duros: sin esto,
   la app no puede desplegarse ni publicarse en las tiendas. Cosas que Claude no puede hacer
   por sí mismo (crear cuentas, pagar, decidir en nombre del equipo) o que requieren
   credenciales/decisiones humanas.
2. **[02-funcionalidades-pendientes.md](./02-funcionalidades-pendientes.md)** — huecos de
   producto: funcionalidades que el documento base o el sentido común piden, pero que aún no
   están construidas (editar/eliminar publicaciones, logo del emprendimiento, moderación,
   vencimiento automático de suscripción, etc.).
3. **[03-calidad-y-riesgos.md](./03-calidad-y-riesgos.md)** — todo lo que se construyó pero
   **no se probó contra datos reales** (solo contra `typecheck`/`lint`/`build` con variables
   de entorno de relleno), más riesgos técnicos y supuestos que el equipo debe validar.
4. **[04-decisiones-del-equipo.md](./04-decisiones-del-equipo.md)** — preguntas que solo el
   equipo (o la universidad) puede responder: naming, marca, precio, responsable legal.

Más una guía práctica, no un listado de huecos:

5. **[05-guia-pruebas-locales.md](./05-guia-pruebas-locales.md)** — cómo pushear el repo a
   GitHub, dar acceso al equipo, y que cualquier colaborador lo clone y pruebe la web en su
   propia máquina contra un proyecto Supabase compartido.

## Cómo usar esta carpeta

- Cuando resuelvas un punto, táchalo o muévelo a un `CHANGELOG.md` si el equipo lleva uno; no
  hace falta mantener esta carpeta sincronizada para siempre, es una foto del estado al
  18 de septiembre de 2026.
- Si retomas el proyecto con Claude Code, pégale el contenido de
  [`CHECKPOINT.md`](./CHECKPOINT.md) (se actualiza automáticamente al final de cada
  respuesta) para que recupere el contexto sin tener que releer todo el historial de chat.
