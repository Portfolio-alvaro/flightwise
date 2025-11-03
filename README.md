# FlightWise

Predicción inteligente de demoras aéreas — aplicación full‑stack (Flask + ML) pensada como proyecto académico y demostración de portfolio.

- Autor: Álvaro García‑Saldaña Carro
- Licencia: MIT

---

## Resumen

FlightWise combina datos de vuelos y meteorología para analizar patrones de puntualidad y predecir minutos de demora estimados. Incluye:

- Backend en Flask que expone una API REST.
- Módulos de Machine Learning organizados (regresión, clustering, clasificación, forecasting).
- Script para generar datos sintéticos y entrenar un modelo (train_model.py).
- Tests básicos con pytest y workflow CI (GitHub Actions).
- Contenedorización con Docker + docker‑compose.

Caso de uso: viajero o responsable operacional que necesita estimar el riesgo de demora para planificar acciones preventivas.

---

## Estructura del repositorio

flightwise/ ├── app/ │ ├── init.py │ ├── routes.py │ ├── models.py │ ├── ml/ │ │ ├── init.py │ │ ├── regression.py │ │ └── clustering.py │ └── utils/ │ ├── init.py │ ├── api_client.py │ └── haversine.py ├── data/ │ ├── vuelos_sample.csv # generado por train_model.py (si no existe) │ └── modelo_regresion.pkl # generado por train_model.py ├── tests/ │ └── test_api.py ├── run.py ├── train_model.py ├── requirements.txt ├── Dockerfile ├── docker-compose.yml ├── .github/workflows/ci.yml ├── .gitignore ├── LICENSE └── README.md
