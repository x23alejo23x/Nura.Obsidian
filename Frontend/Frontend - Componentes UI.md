# Frontend — Componentes UI

Ubicación: `src/components/`

## Layout
| Componente | Descripción |
|------------|-------------|
| `AppShell` | Shell principal con sidebar + header |
| `Sidebar` | Navegación lateral |
| `Header` / `Navbar` | Barra superior |
| `PageContainer` | Wrapper de página con eyebrow, title, actions |

## UI base
| Componente | Descripción |
|------------|-------------|
| `Button` | Variantes: primary, ghost, default |
| `Card` | Card genérico |
| `Input` | Input de texto |
| `Select` | Select nativo; con `searchable` → delega a `Combobox` |
| `Combobox` | Select con búsqueda integrada, navegación por teclado |
| `Table` | Tabla paginada con columnas configurables |
| `Modal` | Modal con header, footer, tamaños |
| `Badge` | Badge de estado |
| `FormField` | Wrapper con label y mensaje de error Zod |

## Charts (Recharts)
| Componente | Uso |
|------------|-----|
| `BarChart` | Series por categoría, agrupadas o stacked |
| `LineChart` | Tendencia, con `dashed` o `area` |
| `DonutChart` | Parte del todo (máx. 5 series) |
| `HBarChart` | Ranking top N |

Paleta y formatters en `src/components/charts/theme.js`.  
**No añadir colores nuevos** — paleta validada para daltonismo.

## Feedback
| Componente | Descripción |
|------------|-------------|
| `ErrorBoundary` | Captura errores de render |
| `Loading` | Spinner genérico |
| `Skeleton` | Placeholder de carga |

## Regla de diseño
- Variables CSS únicamente: `var(--text)`, `var(--surface)`, `var(--border)`
- Gradientes de avatares: `from-blue-500 to-blue-700`, etc.
- Dark mode automático vía `.dark` en `<html>`
