# Changelog

## [Unreleased]

### 🔒 Seguridad
- **Credenciales protegidas**: Movidas credenciales de Gmail de `configuration.yaml` a `secrets.yaml`
  - Antes: Contraseña hardcodeada en texto plano
  - Después: Uso de `!secret` references para mantener credenciales fuera del repositorio
  - ⚠️ Recuerda: `.gitignore` debe excluir `secrets.yaml` en producción

### ⚡ Optimizaciones - Riego
- **Triggers simplificados**: De 48 triggers manuales a automático con `time_pattern`
  - Antes: Listar todas las horas (08:00, 08:20, 08:40, ... 06:00)
  - Después: `hours: "8-19"` y `minutes: "/20"` (ejecuta cada 20 min de 08:00 a 19:59)
  - Beneficio: Código más limpio, más mantenible y menos propenso a errores

- **Lógica de temperatura simplificada**:
  ```yaml
  > 28°C  → 3 minutos
  18-28°C → 2 minutos
  < 18°C  → 1 minuto
  ```

- **Separación de ciclos**:
  - Diurno (08:00-20:00): Cada 20 minutos, duración adaptada
  - Nocturno (20:00-08:00): 1 minuto cada 2 horas

### 📊 Observabilidad
- **Logging del sistema**: Agregado `system_log.write` para debugging
  - Registra cada ciclo ejecutado con temperatura
  - Registra ciclos saltados con motivo
  - Facilita troubleshooting sin acceder a logs de Docker

- **Alertas mejoradas**:
  - Resumen diario con formato visual (emojis, viñetas)
  - Alerta de seguridad con timestamp para ciclos > 6 minutos
  - Mejor contexto en mensajes de error

### 🐛 Incidente documentado
- **Fecha**: 26-05-2026, 14:40-16:40
- **Causa**: API de Open-Meteo (clima) indisponible (~2 horas)
- **Impacto**: Sensor de temperatura pasó a "unavailable", se disparó alerta a las 15:10
- **Fallback**: Sistema continuó regando con valores por defecto (2 min/ciclo)
- **Resolución**: API recuperada automáticamente a las 16:40
- **Lección**: El sistema funciona correctamente con fallbacks. Considerar agregar:
  - [ ] Sensor de temperatura local (ESP32) como alternativa
  - [ ] Valores por defecto más conservadores
  - [ ] Histórico de intentos fallidos en logs

### 📝 Cambios de archivos
- `configuration.yaml`: Credenciales usando `!secret`
- `automations.yaml`: Triggers optimizados, lógica simplificada, logging agregado
- `secrets.yaml`: Nuevas variables para Gmail
- `secrets.yaml.example`: Template actualizado
- `.gitignore`: Asegurar que `secrets.yaml` está excluido

### 🚀 Deployment
- ✅ Commit: `73c8694`
- ✅ Push: `main` branch en GitHub
- ✅ Deploy: `git pull` + `docker compose restart` en ha.binden.es
- ✅ Verificación: Todos los contenedores corriendo sin errores

---

## Próximas mejoras sugeridas
- [ ] Agregar sensor de temperatura del agua (ESP32 + ESPHome)
- [ ] Implementar fallback de API de clima alternativa
- [ ] Sensor de nivel del depósito con alerta
- [ ] Dashboard de histórico de ciclos en Home Assistant
- [ ] Tests automatizados para la lógica de riego
