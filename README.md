# Hidroponico

Proyecto personal de automatización de riego hidropónico usando software libre.

## ¿Qué es esto?

Sistema de riego automatizado para una torre hidropónica vertical con goteo desde arriba y retorno al depósito. Orquesta tres servicios mediante Docker:

- **N8N** — motor de automatización y flujos de trabajo
- **Home Assistant** — control y monitorización del hardware
- **Caddy** — proxy inverso con SSL automático

## Plantas actuales

- 4 pepinos
- 8 tomates cherry
- 2 berenjenas negras
- 6 tomates castellanos
- 2 lechugas
- Hierbabuenas (en incorporación)

## Sistema físico

Torre vertical con arcilla expandida como sustrato de sujeción. El agua gotea desde arriba y refluye al depósito, cerrando el ciclo sin pérdidas.

## Funcionalidades

### Riego adaptativo
- Ciclos cada **20 minutos** de 08:00 a 20:00
- Ciclos cada **2 horas** de 20:00 a 08:00 (1 minuto de duración)
- Duración del ciclo diurno ajustada por temperatura:
  - Mayor de 28°C → 3 minutos
  - Entre 18°C y 28°C → 2 minutos
  - Menor de 18°C → 1 minuto
- Ciclo omitido automáticamente si la humedad supera el 80%

### API del tiempo en tiempo real
Consulta Open-Meteo cada hora con datos de Málaga:
- `sensor.temperatura_malaga` — temperatura ambiente en °C
- `sensor.humedad_malaga` — humedad relativa en %
- `sensor.lluvia_malaga` — precipitación en mm

### Contadores diarios
- Ciclos ejecutados en el día
- Ciclos saltados por humedad alta
- Reset automático a las 00:01

### Notificaciones por email
- **Resumen diario a las 21:00** con temperatura, humedad, lluvia y recuento de ciclos
- **Alerta inmediata** si el enchufe deja de responder más de 10 minutos
- **Alerta** si el sensor del tiempo falla más de 30 minutos

## Estructura del proyecto

```
.
├── docker-compose.yml        # Orquestación de contenedores
├── configuration.yaml        # Config de Home Assistant: sensores, notificaciones, contadores
├── automations.yaml          # Automatizaciones de riego con lógica climática
├── caddy_config/
│   └── Caddyfile             # Proxy inverso con SSL para n8n.binden.es y ha.binden.es
├── install_debian.sh         # Script de instalación en Debian 12
├── .env                      # Variables de entorno (no incluido en el repo)
└── .env.sample               # Plantilla de variables de entorno
```

## Requisitos

- Docker y Docker Compose
- Debian 12 o superior
- Dominio apuntando al servidor
- Copiar `.env.sample` a `.env` y rellenar los valores

## Próximas funcionalidades

- Sensor de temperatura del agua en tiempo real (ESP32 + ESPHome)
- Sensor de nivel del depósito con alerta por email y parada automática de la bomba

## Créditos

El script de instalación base (`install_debian.sh`) fue desarrollado por [Martín Gómez](https://github.com/soymgomez).
