# Frontend — Estructura de páginas

Cada página sigue el mismo patrón (obligatorio per CLAUDE.md):

```
src/pages/NombrePagina/
  Components/
    ComponenteA/index.jsx   ← recibe datos por props
    ComponenteB/index.jsx   ← recibe datos por props
    index.jsx               ← vacío (no re-exportar)
  constants/
    index.js                ← constantes específicas de la página
  services/
    index.js                ← hooks: useEntidad(), useCreateEntidad()…
  View/index.jsx            ← orquestador < 80 líneas
  index.jsx                 ← NO USAR
```

## Reglas clave
- Solo `View/index.jsx` accede a `useAppStore`
- Los sub-componentes reciben datos por props
- `App.jsx` importa desde `pages/X/View`, nunca desde `pages/X/index.jsx`

## Páginas implementadas
| Página | Ruta | Backend conectado |
|--------|------|-------------------|
| AuthView | `/login` | 🔲 JWT pendiente |
| DashboardView | `/dashboard` | Parcial |
| PatientsView | `/pacientes` | ✅ Completo |
| PatientProfileView | `/pacientes/:id` | ✅ Completo |
| MedicationsView | `/medicaciones` | ✅ Completo |
| OwnersView | `/propietarios` | ✅ Completo |
| ReportsView | `/reportes` | 🔲 Datos mock |
| AdminView | `/admin` | 🔲 Mock |
| MedicalHistoryView | — | 🔲 Pendiente |
| FollowUpsView | — | 🔲 Pendiente |

## Página canónica de referencia
`src/pages/Patients/` — ejemplo completo con `constants/`, `services/`, `Components/`, `View/`.
