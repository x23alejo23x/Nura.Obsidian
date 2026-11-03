# Backend — Credenciales y entorno

> ⚠️ Este archivo NO debe subirse a git. Solo existe localmente.

## Variables de entorno — Frontend (`.env.local`)
```
VITE_API_BASE_URL=http://localhost:8080/api
VITE_LOGIN_USER=samayaca
VITE_LOGIN_ROLE=admin
```

## Base de datos PostgreSQL
- Host: `127.0.0.1`
- Puerto: `55432`
- Base de datos: `veterinaria_db`
- Usuario app: `nura_api`
- Contraseña app: `nuraapi123`

> El usuario `postgres` tiene contraseña diferente — no usar directamente.

## Iniciar servidores
```bash
# Backend Laravel
cd C:\MisProgramas\Proyectos\Personal\Veterinaria-api
php artisan serve --port=8080

# Frontend Vite
cd C:\MisProgramas\Proyectos\Personal\Veterinaria-dev
npm run dev   # → http://localhost:6358
```
