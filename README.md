# Hidroponico 🌱

Proyecto personal de automatización de riego hidropónico usando software libre.

## ¿Qué es esto?

Un sistema de riego automatizado que orquesta tres servicios mediante Docker:

- **N8N** — motor de automatización y flujos de trabajo
- **Home Assistant** — control y monitorización del hardware (sensores, actuadores)
- **Caddy** — proxy inverso con SSL automático

## Próximas funcionalidades

- 🌡️ **Sensor de temperatura del agua en tiempo real** — integración con Home Assistant para monitorizar la temperatura del depósito y activar o detener el riego según umbrales configurables
- 🌤️ **API del tiempo** — consulta de previsión meteorológica para que el riego tenga en cuenta la lluvia esperada y la temperatura ambiente, evitando riegos innecesarios

## Estructura del proyecto

```
.
├── docker-compose.yml        # Orquestación de contenedores
├── caddy_config/
│   └── Caddyfile             # Proxy inverso con SSL para n8n.binden.es y ha.binden.es
├── .env                      # Variables de entorno (no incluido en el repo)
└── .env.sample               # Plantilla de variables de entorno
```

## Requisitos

- Docker y Docker Compose
- Un dominio apuntando al servidor
- Copiar `.env.sample` a `.env` y rellenar los valores

## Autor

Proyecto de hobby personal en desarrollo activo.

