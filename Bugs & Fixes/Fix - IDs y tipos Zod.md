# Fix — IDs numéricos y tipos Zod

Fecha: 2026-11-03

## El problema general
El backend devuelve IDs numéricos (`1`, `6`, `12`…).  
Los esquemas Zod del frontend declaran IDs como `z.string().min(1)`.  
Las opciones de Select/Combobox con `value: m.id` (número) no pasan la validación.

## Regla
En cualquier punto donde un ID del backend se pase a:
- `defaultValues` de `useForm`
- `value` de una opción de `<Select>`

→ siempre convertir con `String(...)`.

## En `toPrescriptionPayload` (envío al backend)
El backend espera números en los FK. Convertir de vuelta:
```js
patient_id:    Number(data.patientId    ?? data.patient_id),
medication_id: Number(data.medicationId ?? data.medication_id),
```

## Diagrama del flujo de IDs
```
Backend (número) → normalizePrescription (conserva número)
  → String(m.id) en opciones Select (string para Zod)
  → setValue / defaultValues (string)
  → toPrescriptionPayload → Number(...) (número para backend)
```
