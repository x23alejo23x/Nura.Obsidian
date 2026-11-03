# Consultas — Diseño y Spec

> Spec completa aprobada el 2026-11-03. Ver también [[Consultas - Plan de implementación]].

## Objetivo

Implementar un flujo de consulta clínica con apertura/cierre que permita:
- Gestión completa durante la consulta (notas, medicamentos, exámenes, hospitalización)
- Reporte de cobro en tiempo real y al cierre
- Historia clínica descargable en PDF
- Acceso desde el calendario y desde el perfil del paciente
- Control por rol: médico gestiona, admin puede reabrir consultas cerradas

---

## Modelo de datos

### Tabla `consultas`
```
id, patient_id (FK), appointment_id (FK nullable), vet,
status ENUM(abierta/en_progreso/cerrada),
chief_complaint, diagnosis, base_fee,
opened_at, closed_at, created_at, updated_at
```

### Tabla `exam_catalog`
```
id, name, category, base_price, description, timestamps
```

### Tabla `consulta_exams`
```
id, consulta_id (FK), name, category, unit_price, notes, created_at
```

### Tabla `consulta_hospitalization`
```
id, consulta_id (FK UNIQUE), days, price_per_day, total, notes, timestamps
```

### Modificaciones a `prescriptions`
```
+ consulta_id (FK nullable)
+ unit_price (decimal nullable)
```

---

## Ciclo de vida

```
POST /api/consultas  →  status: "abierta"
  │
  ├─ médico agrega notas/meds/exámenes/hospitalización
  │   └─ status auto → "en_progreso" al primer ítem
  │
  └─ PUT .../close  →  status: "cerrada" — cobro congelado — PDF disponible
       │
       └─ Admin: PUT .../reopen  →  status: "en_progreso"
```

---

## Layout de ConsultaView

```
┌─────────────────────────────────────────────────────┐
│ HEADER: Paciente · Especie · Dr. X · [● Estado]     │
│ Abierta: 03/11/2026 10:30      [Cerrar consulta →]  │
├──────────────┬──────────────────────────────────────┤
│  CobrosPanel │  Tabs: Notas | Medicamentos |        │
│  (sticky)    │        Exámenes | Hospitalización    │
│              │                                      │
│  Consulta    │  Panel activo                        │
│  Meds        │                                      │
│  Exámenes    │                                      │
│  Hosp.       │                                      │
│  ─────────   │                                      │
│  TOTAL       │                                      │
│  [📄 PDF]    │                                      │
└──────────────┴──────────────────────────────────────┘
```

---

## Reglas de negocio

- Solo admin puede reabrir una consulta cerrada
- Al cerrar, los costos quedan congelados (no recalcular)
- Una cita puede tener máximo una consulta activa
- Las prescripciones creadas en consulta heredan `consulta_id`
- El PDF solo está disponible cuando `status === 'cerrada'`
- No hay IVA ni módulo de pagos real (solo reporte)
