# Decisiones que le corresponden al equipo

Preguntas reales que respondí con un valor razonable por defecto para poder seguir avanzando,
pero que no me correspondía decidir de forma definitiva.

| Pregunta | Qué elegí por defecto | Dónde vive |
|---|---|---|
| ¿Responsable legal del tratamiento de datos? | Sin definir — nota explícita en la página | [`apps/web/.../privacidad/page.tsx`](../apps/web/src/app/(legal)/privacidad/page.tsx) |
| ¿Nombre de dominio? | `emprendeunjbg.com` (no comprado) | Varios: `.env.example`, `/privacidad`, `eas.json` |
| ¿Identificador de la app (bundle ID / package name)? | `pe.edu.unjbg.emprende` | [`apps/mobile/app.json`](../apps/mobile/app.json) |
| ¿Colores institucionales? | Azul + dorado genérico (placeholder explícito) | [`packages/ui/src/theme.ts`](../packages/ui/src/theme.ts) |
| ¿Precio de la micro-suscripción mensual? | S/ 10 | [`packages/core/src/constants/suscripcion.ts`](../packages/core/src/constants/suscripcion.ts) |
| ¿Plazo de revisión de un comprobante Yape? | "usualmente 24 horas" (texto, no aplicado técnicamente) | Copy en `/mi-emprendimiento/suscripcion` y `/negocio/suscripcion` |
| ¿Categorías del catálogo? | 10 categorías que propuse yo, basadas en la vida universitaria | [`packages/core/src/constants/categorias.ts`](../packages/core/src/constants/categorias.ts) |
| ¿Correo de contacto para privacidad/soporte? | `privacidad@emprendeunjbg.com` / `soporte@emprendeunjbg.com` (no existen) | `/privacidad`, `/terminos`, `/eliminar-cuenta` |
| ¿Quién revisa cada carpeta del monorepo (CODEOWNERS)? | Placeholders `@equipo-ui`, `@equipo-servidor`, `@equipo-qa` | [`.github/CODEOWNERS`](../.github/CODEOWNERS) |
| ¿La app es un proyecto estudiantil independiente o un servicio oficial de la UNJBG? | Redactado como "proyecto estudiantil del Taller de Emprendimiento" | `/terminos`, `/privacidad`, README |

Ninguno de estos son errores a corregir — son decisiones de producto o institucionales que
necesitan una conversación del equipo (y en algunos casos, de la universidad) antes de
publicar. El código está escrito para que cambiarlos sea barato: son constantes centralizadas,
no valores repetidos por todo el código.
