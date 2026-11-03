# Tareas Frontend — Módulo de Consultas

**Equipo:** Frontend (nosotros)  
**Fecha:** 2026-11-03  
**Depende de:** [[Backend - Tareas Módulo Consultas]] (Tarea 1 mínimo)  
**Ver plan completo:** [[Consultas - Plan de implementación]]

---

## Prerequisito

Esperar a que el Backend termine la **Tarea 1** (endpoints CRUD de consultas) antes de arrancar con la Tarea 4 en adelante. Las Tareas 4-11 no tienen dependencias entre sí más allá del orden indicado.

---

## Tarea 4 — Services + constants

**Estado:** ⬜ Pendiente  
**Archivos:**
- Crear `src/pages/Consultas/services/index.js`
- Crear `src/pages/Consultas/constants/index.js`

**Hooks a exportar:** `useConsultas`, `useConsulta`, `useCreateConsulta`, `useUpdateConsulta`, `useCloseConsulta`, `useReopenConsulta`, `useDeleteConsulta`, `useConsultaExams`, `useAddExam`, `useUpdateExam`, `useDeleteExam`, `useHospitalization`, `useUpsertHospitalization`, `useDeleteHospitalization`

**Patrón:** seguir exactamente `src/pages/Patients/services/index.js`  
**Regla clave:** `onSuccess: () => refetch()` — nunca `onSuccess: refetch`

---

## Tarea 5 — ConsultaView esqueleto + ConsultaHeader + CobrosPanel

**Estado:** ⬜ Pendiente  
**Archivos:**
- Crear `src/pages/Consultas/Components/ConsultaHeader/index.jsx`
- Crear `src/pages/Consultas/Components/CobrosPanel/index.jsx`
- Crear `src/pages/Consultas/View/index.jsx`
- Modificar `src/App.jsx` — agregar rutas `/consultas` y `/consultas/:id`

**CobrosPanel calcula:**
```js
total = baseFee + sum(rx.unitPrice) + sum(exam.unitPrice) + hospitalization.total
```
Formato: `Intl.NumberFormat('es-CO', { style: 'currency', currency: 'COP' })`  
Botón PDF: `disabled` si `status !== 'cerrada'`

---

## Tarea 6 — NotasPanel + MedicamentosPanel

**Estado:** ⬜ Pendiente  
**Archivos:**
- Crear `src/pages/Consultas/Components/NotasPanel/index.jsx`
- Crear `src/pages/Consultas/Components/MedicamentosPanel/index.jsx`

**NotasPanel:** textarea para `chiefComplaint` y `diagnosis`. Read-only si `status === 'cerrada'`.  
**MedicamentosPanel:** reutiliza `PrescriptionModal` existente. Prescripción creada aquí lleva `consulta_id` en el payload.

---

## Tarea 7 — ExamenesPanel + HospitalizacionPanel

**Estado:** ⬜ Pendiente  
**Archivos:**
- Crear `src/pages/Consultas/Components/ExamenesPanel/index.jsx`
- Crear `src/pages/Consultas/Components/HospitalizacionPanel/index.jsx`
- Agregar `useExamCatalog()` en `src/pages/Consultas/services/index.js`

**ExamenesPanel:** examen libre (solo nombre + precio) O del catálogo. Ambos usan el mismo endpoint.  
**HospitalizacionPanel:** `total = days × price_per_day` calculado en tiempo real en UI.

---

## Tarea 8 — ConsultasListView

**Estado:** ⬜ Pendiente  
**Archivos:**
- Crear `src/pages/Consultas/ListView/index.jsx`
- Crear `src/pages/Consultas/Components/ConsultasList/index.jsx`

Botón "Nueva consulta" → modal simple (paciente + vet + base_fee) → al crear redirige a `/consultas/:id`.

---

## Tarea 9 — ConsultaPDF

**Estado:** ⬜ Pendiente  
**Archivos:**
- Crear `src/pages/Consultas/Components/ConsultaPDF/index.jsx`

Componente oculto en pantalla, visible solo con `@media print`. `window.print()` se llama desde `CobrosPanel`.  
Contenido: header clínica, datos consulta, tabla meds, tabla exámenes, hospitalización, resumen cobro, footer.

---

## Tarea 10 — Tab "Consultas" en PatientProfileView

**Estado:** ⬜ Pendiente  
**Archivos:**
- Modificar `src/pages/PatientProfile/constants/index.js` — agregar `'Consultas'` a TABS
- Modificar `src/pages/PatientProfile/View/index.jsx`
- Crear `src/pages/PatientProfile/Components/ConsultasTab/index.jsx`

Al crear consulta desde el perfil → redirige a `/consultas/:newId`.

---

## Tarea 11 — Tab "Exámenes" en MedicationsView

**Estado:** ⬜ Pendiente  
**Archivos:**
- Modificar `src/pages/Medications/View/index.jsx` — tercer tab
- Crear `src/pages/Medications/Components/ExamCatalogTab/index.jsx`
- Modificar `src/pages/Medications/services/index.js` — agregar hooks de ExamCatalog

CRUD completo: crear, editar, eliminar ítems del catálogo.

---

## Notas técnicas importantes

- IDs del backend son numéricos — en Select usar `String(id)`, en payload usar `Number(id)`
- Fechas al backend: `new Date().toISOString().split('T')[0]`
- Variables CSS: `var(--text)`, `var(--surface)`, `var(--border)` — nunca colores hardcoded
- Imports siempre con alias `@/` — nunca `../../`
- Dark mode via clase `.dark` en `<html>` (ya funciona automáticamente)
