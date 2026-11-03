# Rutas y navegación

## Rutas públicas
| Ruta | Vista | Notas |
|------|-------|-------|
| `/login` | `AuthView` | Pública, sin autenticación |

## Rutas protegidas
| Ruta | Vista | Rol requerido |
|------|-------|---------------|
| `/dashboard` | `DashboardView` | — |
| `/pacientes` | `PatientsView` | — |
| `/pacientes/:id` | `PatientProfileView` | — |
| `/medicaciones` | `MedicationsView` | — |
| `/propietarios` | `OwnersView` | — |
| `/reportes` | `ReportsView` | — |
| `/admin` | `AdminView` | `admin` |

## ProtectedRoute
- Sesión en `localStorage` con clave `vet-session`
- `ProtectedRoute` acepta prop `requiredRole`
- Logout: `sessionService.clear()` → redirect a `/login`

## Credenciales de demo
```
VITE_LOGIN_USER=samayaca
VITE_LOGIN_ROLE=admin
```
