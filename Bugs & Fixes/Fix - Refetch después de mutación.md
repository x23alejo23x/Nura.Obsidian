# Fix — Refetch después de mutación no disparaba

Fecha: 2026-11-03

## Síntoma
Después de crear/editar/eliminar un registro, la lista no se actualizaba aunque el toast de éxito aparecía. Requería recargar la página manualmente.

## Causa raíz
`useMutation` llama `onSuccess?.(res)` pasando el cuerpo de la respuesta como primer argumento.  
La función `refetch` (que es `execute` de `useFetch`) tiene firma `execute(overrideUrl, signal)`.  
Al llamar `onSuccess: refetch`, quedaba `refetch(responseBody)` → `overrideUrl = responseBody` (un objeto), no una URL.  
La petición GET se hacía con el objeto como URL y fallaba silenciosamente.

## Fix
Envolver el refetch en una arrow function en todos los views:

```js
// ❌ Antes
useCreatePrescription({ onSuccess: refetchRx })

// ✅ Después
useCreatePrescription({ onSuccess: () => refetchRx() })
```

## Archivos corregidos
- `src/pages/PatientProfile/View/index.jsx` — 3 usos (create, update, delete rx)
- `src/pages/Medications/View/index.jsx` — 4 usos (meds + prescriptions)
- `src/pages/Owners/View/index.jsx` — 3 usos
- `src/pages/Patients/View/index.jsx` — 3 usos

## Nota
Aplica a cualquier futuro hook donde `onSuccess` reciba una función `refetch`. Siempre usar `() => refetch()`.
