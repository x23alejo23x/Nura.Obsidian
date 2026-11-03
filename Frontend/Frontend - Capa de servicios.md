# Frontend — Capa de servicios

## Flujo de datos
```
View/index.jsx
  └─ usePatients()          ← pages/Patients/services/index.js
        └─ useFetch('/patients', { transform: paginated })
              └─ api.js (axios) + apiResponse.js
```

## Hooks globales
- `useFetch(url, options)` → `{ data, loading, error, refetch, reset }`
- `useMutation(method, url, options)` → `{ mutate, loading, error, data }`

Ubicación: `src/hooks/useFetch.js`

## Helpers de respuesta (`src/utils/apiResponse.js`)
| Helper | Descripción |
|--------|-------------|
| `unwrap(res)` | Extrae `res.data.items ?? res.data ?? res` |
| `paginated(res)` | `{ items, total, page, pageSize }` |
| `single(res)` | Objeto único de POST/GET |
| `isEmpty(data)` | true si null / [] / {} |
| `errorMessage(err)` | Mensaje legible del error axios |

## Patrón onSuccess — IMPORTANTE ⚠️
Pasar `refetch` directamente como `onSuccess` es un bug: recibe el body del POST como primer argumento y lo usa como URL override.

```js
// ❌ INCORRECTO
useCreatePrescription({ onSuccess: refetch })

// ✅ CORRECTO
useCreatePrescription({ onSuccess: () => refetch() })
```

Este fix se aplicó en: `PatientProfile/View`, `Medications/View`, `Owners/View`, `Patients/View`.

## Services por página
| Página | Archivo | Hooks exportados |
|--------|---------|-----------------|
| Patients | `pages/Patients/services/index.js` | `usePatients`, `usePatient`, `useCreatePatient`, `useDeletePatient` |
| Owners | `pages/Owners/services/index.js` | `useOwners`, `useOwner`, `useCreateOwner`, `useDeleteOwner` |
| Medications | `pages/Medications/services/index.js` | `useMedications`, `usePrescriptions`, `useCreatePrescription`, `useUpdatePrescription`, `useDeletePrescription`, `toPrescriptionPayload` |
