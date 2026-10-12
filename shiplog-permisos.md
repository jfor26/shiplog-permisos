# ShipLog — Permisos y credenciales necesarios

Resumen de qué accesos hacen falta para desarrollar y desplegar ShipLog, separados por servicio y por fase.

## GitHub

### Para el desarrollo
- **Acceso al repositorio** del proyecto (lectura y escritura) para clonar, crear ramas, hacer push y abrir pull requests.

### Para la Fase 3 (integración de GitHub en el producto)
Cuando ShipLog lea actividad de los repos de los usuarios hará falta una **GitHub App** (o una OAuth App) con permisos mínimos de solo lectura:

| Permiso | Nivel | Para qué |
| --- | --- | --- |
| `Contents` | `read` | Leer commits y archivos del repositorio |
| `Pull requests` | `read` | Leer PRs y su estado para registrarlos como entradas |

No se necesitan permisos de escritura sobre los repos de los usuarios.

## Supabase

### En local
- **Docker** en ejecución (Supabase CLI levanta Postgres, Auth, Mailpit, etc. en contenedores).
- **Node 22**.
- No hacen falta credenciales: `supabase start` genera las claves locales.

### Proyecto en la nube
- Un **proyecto de Supabase** creado.
- Variables de entorno de la app:
  - `NEXT_PUBLIC_SUPABASE_URL`
  - `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- Para aplicar migraciones desde la CLI (`supabase link` / `supabase db push`):
  - **Supabase access token** (personal access token de la cuenta).
  - **Project ref** del proyecto.

## Vercel

- Una **cuenta de Vercel** con el repositorio conectado para el deploy.
- Configurar en el proyecto de Vercel las mismas variables públicas de Supabase:
  - `NEXT_PUBLIC_SUPABASE_URL`
  - `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- Añadir la URL de Vercel a las *Redirect URLs* de Supabase Auth para que funcione el magic link.

## Lo que NO se necesita todavía

- **Proveedor de email externo (p. ej. Resend):** en local los correos de magic link se ven en Mailpit y en la nube basta el SMTP por defecto de Supabase mientras el volumen sea bajo.
- **Credenciales de GitHub para el producto** (GitHub App / OAuth): solo hacen falta cuando empiece la Fase 3.
