# Hidroponico 🌱

Proyecto personal de automatización de riego hidropónico usando software libre.

## ¿Qué es esto?

Un sistema de riego automatizado que orquesta tres servicios mediante Docker:

- **N8N** — motor de automatización y flujos de trabajo
- **Home Assistant** — control y monitorización del hardware (sensores, actuadores)
- **Caddy** — proxy inverso con SSL automático

## Funcionalidades

- ⏱️ **Ciclos de riego adaptativos** por franja horaria (mañana, tarde, noche)
- 🌤️ **API del tiempo en tiempo real** — consulta Open-Meteo cada hora con datos de Málaga:
  - Temperatura ambiente → ajusta la duración del riego (1, 2 o 3 minutos)
  - Humedad relativa → si supera el 80%, el ciclo se omite automáticamente
  - Lluvia — dato informativo en el dashboard
- 🔌 **Control del enchufe inteligente** vía Home Assistant

## Lógica de riego

| Temperatura | Humedad | Duración |
|---|---|---|
| > 28°C | < 80% | 3 minutos |
| 18–28°C | < 80% | 2 minutos |
| < 18°C | < 80% | 1 minuto |
| cualquiera | ≥ 80% | ciclo omitido |

La franja nocturna (20:00–08:00) riega siempre 1 minuto independientemente del clima.

## Plantas actuales

- 2 berenjenas negras
- 2 pepinos
- 6 tomates castellanos
- 2 tomates cherry
- 2 lechugas
- Hierbabuenas (en incorporación)

## Próximas funcionalidades

- 🌡️ **Sensor de temperatura del agua en tiempo real** — integración con Home Assistant para monitorizar la temperatura del depósito y ajustar el riego según umbrales configurables

## Estructura del proyecto

```
.
├── docker-compose.yml        # Orquestación de contenedores
├── configuration.yaml        # Configuración de Home Assistant con sensores meteorológicos
├── automations.yaml          # Automatizaciones de riego con lógica climática
├── caddy_config/
│   └── Caddyfile             # Proxy inverso con SSL
├── .env                      # Variables de entorno (no incluido en el repo)
└── .env.sample               # Plantilla de variables de entorno
```

## Requisitos

- Docker y Docker Compose
- Un dominio apuntando al servidor
- Copiar `.env.sample` a `.env` y rellenar los valores

## Créditos

El script de instalación base (`install_debian.sh`) fue desarrollado por [Martín Gómez](https://github.com/soymgomez).

