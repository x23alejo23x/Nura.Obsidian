# Pendientes generales

Última actualización: 2026-11-03

---

## 🔴 Alta prioridad — Gestión clínica completa del paciente

### 1. Historia clínica descargable
- Consolidar en un solo documento PDF todo el historial del paciente:
  - Notas clínicas
  - Prescripciones
  - Exámenes
  - Hospitalizaciones
- Debe ser descargable desde el perfil del paciente
- Generado por registro y por rango de fechas

### 2. Asignación de medicamentos y exámenes por el médico
- El médico debe poder asignar desde la consulta:
  - Medicamentos (con dosis, frecuencia, costo)
  - Exámenes (con tipo, descripción, costo)
  - Hospitalización (días, costo por día)
- Todo esto debe quedar en la historia clínica

### 3. Reporte de cobro por consulta
- Al iniciar una consulta se genera un ticket que acumula:
  - Costo de consulta inicial
  - Costo de medicamentos recetados
  - Costo de exámenes ordenados
  - Costo de hospitalización
- El reporte debe ser visible y descargable (PDF)
- Debe poder ser editado por el médico antes de cerrar la consulta

### 4. Calendario de controles
- Registrar seguimientos / controles en un calendario
- Filtrar por paciente
- Ver espacios disponibles globalmente
- Asignar hora a propietario/paciente

---

## 🟡 Media prioridad — Backend pendiente de conectar

| Módulo | Archivo frontend | Tarea |
|--------|-----------------|-------|
| MedicalHistory | `pages/MedicalHistory/View/index.jsx` | Crear endpoint + conectar |
| FollowUps | `pages/FollowUps/View/index.jsx` | Crear endpoint + conectar |
| Reports | `pages/Reports/View/index.jsx` | Conectar datos reales |
| Auth JWT | `services/auth.js` | Reemplazar login mock por JWT real |
| ProtectedRoute | `router/` | Activar validación real de sesión |

---

## 🟢 Baja prioridad — Mejoras y pulido

- [ ] Probar cambio de estado de prescripción (Activa → Finalizada → Suspendida)
- [ ] Probar editar prescripción existente
- [ ] Probar eliminar prescripción
- [ ] Vista `/medicaciones` tab Prescripciones mostrando todas
- [ ] CRUD de medicamentos con refetch
- [ ] Eliminar seed data de `appStore.js` progresivamente por entidad
- [ ] Datos `concentration` de medicamentos en backend (actualmente null)
- [ ] Dashboard con datos reales

---

## ✅ Completado

- [x] Endpoint `/api/prescriptions` (DDD completo en backend)
- [x] Flujo create prescription end-to-end
- [x] Fix: fecha ISO en prescripciones
- [x] Fix: refetch después de mutación en 4 vistas
- [x] Fix: IDs numéricos vs string Zod
- [x] Fix: snake_case normalización en prescripciones
- [x] Fix: "undefined ml" en dropdown medicamentos
- [x] Pacientes, Propietarios, Medicamentos conectados al backend
- [x] PatientProfileView carga datos reales del backend
