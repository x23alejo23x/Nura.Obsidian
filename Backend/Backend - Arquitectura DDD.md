# Backend — Arquitectura DDD

Proyecto: `C:\MisProgramas\Proyectos\Personal\Veterinaria-api`

## Capas
```
Domain/
  Entities/         ← entidades de negocio puras
  Repositories/     ← interfaces (contratos)

Application/
  UseCases/         ← lógica de negocio (Create, Update, Delete, List)

Infrastructure/
  Eloquent/         ← implementaciones con modelos Laravel
  Repositories/     ← repositorios concretos

Presentation/
  Controllers/      ← endpoints HTTP
  Resources/        ← transformadores JSON (API Resources)
  Requests/         ← validación de entrada (Form Requests)
```

## Módulos implementados
| Módulo | Estado |
|--------|--------|
| Patients | ✅ Completo |
| Owners | ✅ Completo |
| Medications | ✅ Completo |
| Prescriptions | ✅ Completo |
| MedicalHistory | 🔲 Pendiente conectar |
| FollowUps | 🔲 Pendiente conectar |

## Convención snake_case
El backend devuelve **snake_case**. El frontend normaliza a camelCase en la capa de servicios.

Ejemplo en `Medications/services/index.js`:
```js
function normalizePrescription(rx) {
  return {
    ...rx,
    medicationId: rx.medication_id ?? rx.medicationId,
    patientId:    rx.patient_id   ?? rx.patientId,
  }
}
```

## Formato de fechas
El backend espera fechas en formato **ISO 8601**: `YYYY-MM-DD`.  
❌ No usar `toLocaleDateString()` — genera formatos como `"03 nov. 2026"` que Laravel rechaza.  
✅ Usar `new Date().toISOString().split('T')[0]`
