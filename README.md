# FlightWise

Predicción inteligente de demoras aéreas — aplicación full‑stack (Flask + ML) pensada como proyecto académico y demostración de portfolio.

- Autor: Álvaro García‑Saldaña Carro
- Licencia: MIT

---Crea y activa un entorno virtual:

bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
Instala dependencias:

bash
pip install --upgrade pip
pip install -r requirements.txt
Genera datos de ejemplo y entrena el modelo:

bash
python train_model.py
Esto genera data/vuelos_sample.csv y serializa el mejor modelo en data/modelo_regresion.pkl.

Ejecuta la aplicación:

bash
python run.py
Accede a http://127.0.0.1:5000

Probar endpoints (curl):

bash
curl http://127.0.0.1:5000/api/health

curl -X POST http://127.0.0.1:5000/api/predict/delay \
  -H "Content-Type: application/json" \
  -d '{
    "origen": {"lat": 40.472, "lon": -3.561},
    "destino": {"lat": 51.470, "lon": -0.454},
    "viento": 12.5,
    "temperatura": 10
  }'
Arquitectura de la aplicación
run.py: entrada principal que crea y arranca la app Flask.

app/: código de la aplicación

routes.py: endpoints REST

models.py: placeholder para ORM/persistencia

ml/: módulos de Machine Learning (regresión, clustering)

utils/: utilidades (haversine, cliente API)

data/: datos y modelos serializados

tests/: pruebas pytest

Dockerfile / docker-compose.yml: despliegue local por contenedores

.github/workflows/: CI (lint + pytest) — opcional si se añade

Diseño: separación entre API (routes), lógica ML (app/ml) y utilidades (app/utils) para facilitar pruebas y despliegue.

Documentación de los endpoints de la API
GET /api/health

Qué hace: comprobación de estado (health check).

Recibe: nada.

Devuelve: JSON {"status":"ok"}.

GET /api/data/summary

Qué hace: devuelve un resumen mínimo/placeholder con información del dataset y si existe el modelo.

Recibe: parámetros por query opcionales (no implementados en el esqueleto).

Devuelve: JSON con keys como "n_vuelos_sample" y "modelo_existe".

POST /api/predict/delay

Qué hace: calcula distancia entre origen y destino, carga el modelo serializado y devuelve una predicción de minutos de demora.

Recibe: JSON con la estructura:

json
{
  "origen": {"lat": 40.472, "lon": -3.561},
  "destino": {"lat": 51.470, "lon": -0.454},
  "viento": 12.5,
  "temperatura": 10
}
Devuelve: JSON con:

json
{
  "distancia_km": 1259.342,
  "prediccion_demora_min": 12.34
}
Errores comunes: 400 si JSON malformado; 500 si modelo no existe.

Resumen de hallazgos y conclusiones del análisis
Con datos sintéticos generados por train_model.py se puede entrenar y evaluar modelos básicos (Linear, Ridge, Lasso). El script selecciona el modelo con menor RMSE en el test.

En el dataset sintético la demora depende linealmente de distancia y viento (según la fórmula generadora), por lo que regresión lineal rinde bien.

:

Añadir variables categóricas (aerolínea, tipo de avión, hora del día, día de la semana).

Modelado por etapas: clasificación de riesgo (p.ej. demora > 15 min) + regresión condicionada para minutos.

Pipeline reproducible con preprocesado (scikit-learn Pipeline), validación cruzada y selección de características.

Monitorizar deriva del modelo y recalibrar periódicamente.

Complementar con forecasting para patrones estacionales en tráfico.

Conclusión: el esqueleto permite pruebas rápidas y despliegue inicial; para calidad de producción es necesario enriquecer datos, robustecer la ingesta y añadir control de versiones para los modelos

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
