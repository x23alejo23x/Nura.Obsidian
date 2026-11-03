# Stack y arquitectura

## Frontend
| Tech | Versión | Uso |
|------|---------|-----|
| React | 19 | UI principal |
| Vite | 8 | Bundler + HMR |
| Tailwind CSS | v4 | Estilos (con `@tailwindcss/vite`) |
| HeroUI | v3 | Componentes UI (`@heroui/react`) |
| Zustand | 5 | Estado global (`src/store/appStore.js`) |
| react-hook-form + Zod | — | Formularios y validación |
| framer-motion | — | Animaciones |
| react-hot-toast | — | Notificaciones (`src/utils/notify.js`) |
| axios | — | HTTP client (`src/services/api.js`) |
| React Router DOM | v7 | Navegación |
| Recharts | — | Gráficos (wrappers en `src/components/charts/`) |

## Backend
| Tech | Uso |
|------|-----|
| Laravel | Framework PHP principal |
| PostgreSQL | Base de datos |
| Arquitectura DDD | Domain → Application → Infrastructure → Presentation |

## Alias de imports
Todos los imports usan `@` apuntando a `src/`:
```js
import { Button } from '@/components/ui/Button'
import { useAppStore } from '@/store/appStore'
```

## Variables CSS
Los componentes usan siempre variables CSS, **nunca colores hardcoded**:
- `var(--text)` `var(--text-2)` `var(--text-3)`
- `var(--surface)` `var(--surface-2)`
- `var(--border)`

## Dark mode
Vía clase `.dark` en `<html>` — `ThemeContext` lo maneja.
