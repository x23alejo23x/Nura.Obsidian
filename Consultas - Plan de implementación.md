# Consultas — Plan de implementación

> Plan completo en el repo: `docs/superpowers/plans/2026-11-03-consultas.md`  
> Ver también [[Consultas - Diseño y spec]]

## Resumen de tareas

| # | Tarea | Equipo | Estado |
|---|-------|--------|--------|
| 1 | Backend: Dominio Consulta + CRUD + close/reopen | Backend | ⬜ Pendiente |
| 2 | Backend: ExamCatalog + ConsultaExam + Hospitalization | Backend | ⬜ Pendiente |
| 3 | Backend: Modificar Prescription (consulta_id + unit_price) | Backend | ⬜ Pendiente |
| 4 | Frontend: services + constants de Consultas | Frontend | ⬜ Pendiente |
| 5 | Frontend: ConsultaView + ConsultaHeader + CobrosPanel | Frontend | ⬜ Pendiente |
| 6 | Frontend: NotasPanel + MedicamentosPanel | Frontend | ⬜ Pendiente |
| 7 | Frontend: ExamenesPanel + HospitalizacionPanel | Frontend | ⬜ Pendiente |
| 8 | Frontend: ConsultasListView (lista global) | Frontend | ⬜ Pendiente |
| 9 | Frontend: ConsultaPDF + @media print | Frontend | ⬜ Pendiente |
| 10 | Frontend: Tab Consultas en PatientProfileView | Frontend | ⬜ Pendiente |
| 11 | Frontend: Tab Exámenes en MedicationsView | Frontend | ⬜ Pendiente |
| 12 | Obsidian vault + commit final | Ambos | ⬜ Pendiente |

---

## Dependencias

```
Backend Task 1  →  Frontend Tasks 4-11 (necesitan los endpoints)
Backend Task 2  →  Frontend Tasks 7, 11
Backend Task 3  →  Frontend Task 6 (unit_price en prescripciones)
```

**Coordinación:** Cuando el Backend termine la Tarea 1, avisar al Frontend para conectar los hooks y probar end-to-end.

---

## Archivos frontend a crear

```
src/pages/Consultas/
  Components/
    ConsultaHeader/index.jsx
    NotasPanel/index.jsx
    MedicamentosPanel/index.jsx
    ExamenesPanel/index.jsx
    HospitalizacionPanel/index.jsx
    CobrosPanel/index.jsx
    ConsultaPDF/index.jsx
    ConsultasList/index.jsx
  services/index.js
  constants/index.js
  View/index.jsx
  ListView/index.jsx
```

**Modificaciones:**
- `src/pages/PatientProfile/View/index.jsx` — tab "Consultas"
- `src/pages/PatientProfile/constants/index.js` — agregar 'Consultas' a TABS
- `src/pages/Medications/View/index.jsx` — tab "Exámenes"
- `src/pages/Medications/services/index.js` — hooks de ExamCatalog
- `src/App.jsx` — rutas `/consultas` y `/consultas/:id`

---

## Archivos backend a crear

```
app/Domain/Consulta/            (Entities, DTOs, Repositories)
app/Domain/ExamCatalog/         (Entities, DTOs, Repositories)
app/Application/Consulta/UseCases/   (7 use cases)
app/Application/ExamCatalog/UseCases/ (5 use cases)
app/Infrastructure/.../Models/  (Consulta, ExamCatalog, ConsultaExam, ConsultaHospitalization)
app/Infrastructure/.../Repositories/ (EloquentConsulta, EloquentExamCatalog)
app/Presentation/Http/Controllers/Api/ (Consulta, ExamCatalog, ConsultaExam, ConsultaHospitalization)
app/Presentation/Http/Resources/ (4 resources)
app/Presentation/Http/Requests/  (Consulta requests)
database/migrations/             (4 migraciones nuevas)
```

Ver detalle completo: [[Backend - Tareas Módulo Consultas]]
