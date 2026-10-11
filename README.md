# 🍔 Proyecto ETL & Inteligencia de Mercado — Food Delivery

Trabajo Práctico Integrador · Python para Análisis de Datos (ETL) · Data Academy Latam

Pipeline completo **Extract → Transform → Load → BI** para ayudar a una startup de Food Delivery a decidir en qué ciudades de EE. UU. invertir.

## 📁 Estructura
```
.
├── TPI_ETL_Delivery.ipynb      # Cuaderno principal (ya ejecutado, con gráficos)
├── delivery_insights.db        # Base SQLite generada (tabla master_food_data)
├── data/
│   ├── restaurants.csv         # Catálogo de restaurantes (~63 mil filas)
│   └── restaurant-menus/       # Menús (~836 mil ítems) en 10 partes
├── requirements.txt
└── README.md
```

## ▶️ Cómo ejecutarlo
**Google Colab:** subí el repo (o `!git clone <URL>` y `%cd <carpeta>`) y ejecutá todas las celdas.
**Local:** `pip install -r requirements.txt` → `jupyter notebook TPI_ETL_Delivery.ipynb`.

## 🔄 Resumen del pipeline
| Fase | Resumen |
|---|---|
| 📥 Extract | Lectura de ambos CSV (menús en 10 partes concatenadas) + profiling de shape, dtypes y nulos |
| 🧹 Transform | Mapeo de precios (`$`→texto), split de dirección en `calle / ciudad / estado_zip`, filtro de score nulo/0, categorías en minúsculas, casteo de precio a `float`, descarte de precios ≤ 0 y duplicados, filtro `ratings > 100` e INNER JOIN → `df_master` |
| 💾 Load | SQLAlchemy + SQLite → `delivery_insights.db`, tabla `master_food_data` (63.186 filas, 671 restaurantes) |
| 📊 BI | 4 preguntas del CEO resueltas con SQL + gráficos + conclusión |

## 🔑 Resultados clave
1. **Ciudades líderes:** Milwaukee (123) y Seattle (119) concentran más restaurantes consolidados.
2. **Precios:** el mercado es mayormente *Económico* (67,1 %); solo 0,6 % es *Caro*.
3. **Precio promedio del plato:** Seattle USD 11,97 > Lynnwood USD 10,76 > Milwaukee USD 9,53.
4. **Precio ≠ calidad:** el puntaje no mejora al subir de categoría de precio.

## 📝 Decisiones de datos
- Los CSV usan columnas en inglés; se renombraron a las del enunciado (`rango_de_precios`, `direccion_completa`, `precio`).
- Se usó `.str.rsplit(',', n=3)` (variante de `split`) porque algunas calles contienen comas.
- Como `master_food_data` tiene una fila por plato, las métricas por restaurante usan `COUNT(DISTINCT id)` / `SELECT DISTINCT`.
- Además de precios 0/nulos se descartaron 2 precios negativos y 2.537 filas duplicadas.
