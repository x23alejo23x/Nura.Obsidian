# Backend — Endpoints disponibles

Base URL: `http://localhost:8080/api`

## Patients `/patients`
| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/patients` | Listar (soporta `?search=`, `?species=`, `?page=`, `?limit=`) |
| GET | `/patients/:id` | Obtener uno |
| POST | `/patients` | Crear |
| PUT | `/patients/:id` | Actualizar |
| DELETE | `/patients/:id` | Eliminar |

## Owners `/owners`
| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/owners` | Listar |
| GET | `/owners/:id` | Obtener uno |
| POST | `/owners` | Crear |
| PUT | `/owners/:id` | Actualizar |
| DELETE | `/owners/:id` | Eliminar |

## Medications `/medications`
| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/medications` | Listar (soporta `?search=`, `?limit=`) |
| POST | `/medications` | Crear |
| PUT | `/medications/:id` | Actualizar |
| DELETE | `/medications/:id` | Eliminar |

## Prescriptions `/prescriptions`
| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/prescriptions` | Listar (soporta `?patient_id=`) |
| POST | `/prescriptions` | Crear |
| PUT | `/prescriptions/:id` | Actualizar |
| DELETE | `/prescriptions/:id` | Eliminar |

## Payload de prescripción
```json
{
  "patient_id": 6,
  "medication_id": 1,
  "dose": "500 mg",
  "frequency": "cada 12 horas",
  "duration": "7 días",
  "notes": null,
  "status": "Activa",
  "vet": null,
  "date": "2026-11-03"
}
```

> ⚠️ La fecha debe ir en formato `YYYY-MM-DD` (ISO). Ver [[Backend - Arquitectura DDD]].
