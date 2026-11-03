# Fix — Prescripciones (flujo completo)

Fecha: 2026-11-03

## Problema original
La gestión de prescripciones desde el perfil del paciente fallaba con error 404 en `/api/prescriptions`.

## Causa
El endpoint no existía en el backend. Se creó el módulo DDD completo en Laravel.

## Solución
Crear en `Veterinaria-api`:
- `Domain/Prescription/` → entidad + interfaz repositorio
- `Application/Prescription/UseCases/` → Create, Update, Delete, List
- `Infrastructure/Eloquent/PrescriptionRepository.php`
- `Presentation/Controllers/PrescriptionController.php`
- `Presentation/Resources/PrescriptionResource.php`
- `Presentation/Requests/StorePrescriptionRequest.php`
- Migración + modelo Eloquent + registro en rutas

---

## Fix — IDs numéricos vs strings en Zod
**Síntoma:** El formulario no se enviaba sin mostrar error visible.  
**Causa:** `patient.id` del backend es número, pero `prescriptionSchema` tiene `patientId: z.string().min(1)`.  
**Fix:** `String(patient?.id ?? '')` en `defaultValues` del form.  
Archivo: `PatientProfile/Components/PrescriptionModal/index.jsx`

---

## Fix — Opciones de medicamento con valor numérico
**Síntoma:** Zod rechazaba `medicationId` con "Elige el tipo…".  
**Causa:** `value: m.id` pasaba número, Zod requiere string.  
**Fix:** `value: String(m.id)` en el map de opciones.

---

## Fix — "undefined ml" en dropdown de medicamentos
**Síntoma:** El label mostraba `"Amoxicilina — undefined tabletas"`.  
**Causa:** `m.concentration` es null/undefined en algunos registros del backend.  
**Fix:** Guard en template literal (el campo `unit` es el que muestra, no concentration).

---

## Fix — snake_case vs camelCase en prescripciones
**Síntoma:** `medications.find((m) => m.id === item.medicationId)` retornaba undefined.  
**Causa:** Backend devuelve `medication_id` y `patient_id`, frontend leía `medicationId` y `patientId`.  
**Fix:** Normalización en `usePrescriptions`:
```js
function normalizePrescription(rx) {
  return { ...rx, medicationId: rx.medication_id ?? rx.medicationId, patientId: rx.patient_id ?? rx.patientId }
}
```
Archivo: `Medications/services/index.js`
