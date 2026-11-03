# Tareas Backend — Módulo de Consultas

**Asignado a:** Equipo Backend  
**Fecha:** 2026-11-03  
**Plan completo:** [Ver plan de implementación](https://github.com/x23alejo23x/Veterinaria-dev/blob/Preproduccion/docs/superpowers/plans/2026-11-03-consultas.md)  
**Spec de diseño:** [Ver spec](https://github.com/x23alejo23x/Veterinaria-dev/blob/Preproduccion/docs/superpowers/specs/2026-11-03-consultas-design.md)  
**Repositorio API:** https://github.com/x23alejo23x/Veterinaria-api.git  
**Rama de trabajo sugerida:** `feature/consultas`

---

## Contexto

El frontend (Veterinaria-dev) ya está preparado para consumir estos endpoints.  
La arquitectura del proyecto sigue **DDD** (ver [[Backend - Arquitectura DDD]]).  
Para cada nuevo dominio, seguir exactamente el patrón de `Prescription` que ya existe en el repo.

---

## Patrón de referencia — Prescription

Cada dominio nuevo necesita estos archivos (copiar estructura de Prescription):

```
app/Domain/NuevoDominio/
  Entities/NuevoDominio.php          ← entidad inmutable (readonly)
  DTOs/NuevoDominioDTO.php           ← fromArray(snake_case → camelCase)
  Repositories/NuevoDominioRepositoryInterface.php

app/Application/NuevoDominio/UseCases/
  ListNuevoDominio.php
  GetNuevoDominio.php
  CreateNuevoDominio.php
  UpdateNuevoDominio.php
  DeleteNuevoDominio.php

app/Infrastructure/Persistence/Eloquent/
  Models/NuevoDominio.php            ← Eloquent model + $fillable + $casts
  Repositories/EloquentNuevoDominioRepository.php

app/Presentation/Http/
  Controllers/Api/NuevoDominioController.php
  Requests/NuevoDominio/StoreNuevoDominioRequest.php
  Requests/NuevoDominio/UpdateNuevoDominioRequest.php
  Resources/NuevoDominioResource.php

app/Infrastructure/Providers/RepositoryServiceProvider.php  ← agregar binding
routes/api.php                                              ← agregar ruta
```

---

## Tarea 1 — Dominio Consulta + CRUD + Ciclo de vida

**Prioridad:** 🔴 Alta — el frontend depende de esto primero  
**Estado:** ⬜ Pendiente

### Migración `consultas`

```sql
id               BIGINT PK AUTO_INCREMENT
patient_id       FK → patients (cascade delete)
appointment_id   FK → appointments (nullable)
vet              VARCHAR(255)
status           ENUM('abierta', 'en_progreso', 'cerrada') DEFAULT 'abierta'
chief_complaint  TEXT NULL
diagnosis        TEXT NULL
base_fee         DECIMAL(10,2) DEFAULT 0
opened_at        TIMESTAMP DEFAULT now()
closed_at        TIMESTAMP NULL
created_at, updated_at
```

### Endpoints a implementar

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/api/consultas` | Listar — filtros: `patient_id`, `status`, `date_from`, `date_to` |
| GET | `/api/consultas/:id` | Obtener una consulta |
| POST | `/api/consultas` | Crear (status inicial: `abierta`) |
| PUT | `/api/consultas/:id` | Actualizar datos |
| POST | `/api/consultas/:id/close` | Cerrar → status `cerrada`, registra `closed_at` |
| POST | `/api/consultas/:id/reopen` | Reabrir → status `en_progreso` (**solo admin**) |
| DELETE | `/api/consultas/:id` | Eliminar (**solo si está `abierta`**) |

### Reglas de negocio importantes

- `ReopenConsulta` use case debe lanzar excepción 403 si `auth()->user()->role !== 'admin'`
- Si se pasa `appointment_id`, validar que esa cita no tenga ya una consulta `abierta` o `en_progreso`
- Al crear, `opened_at = now()`, `closed_at = null`
- Al cerrar, `closed_at = now()`

### Respuesta esperada (ConsultaResource)

```json
{
  "data": {
    "id": 1,
    "patient_id": 3,
    "appointment_id": null,
    "vet": "Dr. García",
    "status": "abierta",
    "chief_complaint": "Pérdida de apetito",
    "diagnosis": null,
    "base_fee": "50000.00",
    "opened_at": "2026-11-03T10:30:00",
    "closed_at": null,
    "created_at": "2026-11-03T10:30:00"
  }
}
```

### Rutas en `api.php`

```php
Route::apiResource('consultas', ConsultaController::class);
Route::post('consultas/{id}/close',  [ConsultaController::class, 'close']);
Route::post('consultas/{id}/reopen', [ConsultaController::class, 'reopen']);
```

---

## Tarea 2 — ExamCatalog + ConsultaExam + ConsultaHospitalization

**Prioridad:** 🔴 Alta  
**Estado:** ⬜ Pendiente  
**Depende de:** Tarea 1 (necesita `consultas` table)

### Migraciones

**`exam_catalog`**
```sql
id          BIGINT PK
name        VARCHAR(255)
category    VARCHAR(255)
base_price  DECIMAL(10,2)
description TEXT NULL
created_at, updated_at
```

**`consulta_exams`**
```sql
id          BIGINT PK
consulta_id FK → consultas (cascade delete)
name        VARCHAR(255)      -- texto libre o del catálogo
category    VARCHAR(255) NULL
unit_price  DECIMAL(10,2)     -- editable, puede diferir del catálogo
notes       TEXT NULL
created_at                    -- sin updated_at
```

**`consulta_hospitalization`**
```sql
id            BIGINT PK
consulta_id   FK → consultas (UNIQUE — máximo 1 por consulta)
days          INT
price_per_day DECIMAL(10,2)
total         DECIMAL(10,2)   -- calcular: days × price_per_day
notes         TEXT NULL
created_at, updated_at
```

### Endpoints ExamCatalog

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/api/exam-catalog` | Listar catálogo |
| POST | `/api/exam-catalog` | Crear |
| PUT | `/api/exam-catalog/:id` | Editar |
| DELETE | `/api/exam-catalog/:id` | Eliminar |

### Endpoints ConsultaExam (nested bajo consulta)

| Método | Ruta | Descripción |
|--------|------|-------------|
| POST | `/api/consultas/:id/exams` | Agregar examen a consulta |
| PUT | `/api/consultas/:id/exams/:examId` | Editar examen |
| DELETE | `/api/consultas/:id/exams/:examId` | Eliminar examen |

### Endpoints ConsultaHospitalization

| Método | Ruta | Descripción |
|--------|------|-------------|
| POST | `/api/consultas/:id/hospitalization` | Crear o actualizar (upsert) |
| DELETE | `/api/consultas/:id/hospitalization` | Eliminar |

### Notas

- `total` en hospitalización se calcula en el controller: `days × price_per_day`
- El examen puede ser libre (sin catálogo) o del catálogo — el campo `name` siempre es texto
- El `unit_price` del examen es siempre editable (puede diferir del `base_price` del catálogo)

---

## Tarea 3 — Modificar tabla `prescriptions` (backward compatible)

**Prioridad:** 🟡 Media  
**Estado:** ⬜ Pendiente  
**Depende de:** Tarea 1 (necesita `consultas` table)

### Migración aditiva

```php
// Agregar dos columnas opcionales — no rompe nada existente
$table->unsignedBigInteger('consulta_id')->nullable()->after('patient_id');
$table->foreign('consulta_id')->references('id')->on('consultas')->nullOnDelete();
$table->decimal('unit_price', 10, 2)->nullable()->after('duration');
```

### Archivos a modificar

- `Models/Prescription.php` — agregar `'consulta_id'`, `'unit_price'` a `$fillable` y `$casts`
- `Entities/Prescription.php` — agregar `?int $consultaId` y `?float $unitPrice` al constructor
- `DTOs/PrescriptionDTO.php` — agregar campos opcionales
- `Requests/Prescription/StorePrescriptionRequest.php` — agregar `'consulta_id' => 'nullable|integer|exists:consultas,id'`
- `Resources/PrescriptionResource.php` — exponer `consulta_id` y `unit_price`

### Verificación

Los registros de prescripciones existentes deben retornar `"consulta_id": null, "unit_price": null` sin error.

---

## Checklist de entrega

- [ ] Tarea 1: endpoints de consultas funcionando con Postman/curl
- [ ] Tarea 1: `POST /api/consultas/:id/reopen` retorna 403 si role ≠ admin
- [ ] Tarea 2: CRUD exam-catalog funcionando
- [ ] Tarea 2: examen libre (sin catálogo) se puede agregar con solo `name` y `unit_price`
- [ ] Tarea 2: hospitalización hace upsert (no duplica si ya existe)
- [ ] Tarea 3: `GET /api/prescriptions/1` retorna `consulta_id: null` sin romper
- [ ] Todas las rutas bajo `Route::middleware('auth:sanctum')`
- [ ] Bindings registrados en `RepositoryServiceProvider`
- [ ] Migrations corren sin error en base limpia (`php artisan migrate:fresh`)

---

## Variables de entorno necesarias

El `.env` local debe tener (ver [[Backend - Credenciales y entorno]]):
```
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=55432
DB_DATABASE=veterinaria_db
DB_USERNAME=nura_api
DB_PASSWORD=nuraapi123
```

---

## Comunicación con el equipo frontend

Una vez que **Tarea 1** esté lista (aunque sea sin Tarea 2 y 3), avisar al equipo frontend para conectar los hooks de `useConsultas` y probar el flujo end-to-end.

El frontend espera la respuesta en el formato de `ConsultaResource` detallado arriba.
