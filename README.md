# Aprendizaje No Supervisado — Taller Práctico

Dos datasets. Dos enfoques. Una misma pregunta: ¿qué estructura tienen los datos cuando no hay etiqueta que guíe el análisis?

---

## Descripción del proyecto

Este taller explora técnicas de **aprendizaje no supervisado** aplicadas a dos datasets con características muy distintas:

- **Setas** — datos categóricos con etiqueta disponible, usada únicamente para validar a posteriori.
- **Tarjetas de crédito** — datos financieros numéricos sin etiqueta, donde el objetivo es segmentar clientes para negocio.

La diferencia clave entre ambos casos es pedagógica: en setas se puede medir cuánta estructura real recupera el clustering sin haberla visto; en tarjetas no existe esa referencia y el éxito se mide por métricas internas y por la utilidad de los segmentos.

---

## Estructura del repositorio

```
.
├── data/
│   ├── mushrooms.csv
│   └── credit_card.csv
├── workshop-clustering-Mushrooms.ipynb
├── workshop-clustering-creditcard.ipynb
└── README.md
```

---

## Parte 1 — Setas (datos categóricos, con etiqueta)

**Notebook:** `workshop-clustering-Mushrooms.ipynb` · **Dataset:** `data/mushrooms.csv`

8.124 hongos descritos con 22 variables categóricas. La variable `class` (comestible / venenosa) **no se usa en el clustering** — solo se reserva para validar al final.

### Qué se trabajó

- EDA: detección de nulos encubiertos (`'?'` en `stalk-root`) y columna constante (`veil-type`).
- Preprocesado: imputación con la moda y **One-Hot Encoding** → 115 dimensiones.
- **PCA** y **t-SNE** para visualizar en 2D.
- **Random Forest** como línea base supervisada: accuracy **1.0000** con todas las features.
- Estudio de cuántas componentes PCA bastan para mantener esa precisión.
- Clustering: **K-Means**, **Aglomerativo**, **GMM** y **DBSCAN** (euclídea vs Jaccard).
- Validación con etiqueta: **ARI** y **NMI**.
- **Isolation Forest** para detección de anomalías.

### Resultados principales

| Algoritmo | ARI | NMI |
|---|---|---|
| K-Means (k=2) | 0.617 | 0.566 |
| Aglomerativo | — | — |
| GMM | — | — |
| DBSCAN (euclídea) | menor que K-Means | — |

> El dataset tiene estructura categórica muy marcada. K-Means recupera buena parte de la clase real sin haberla visto. DBSCAN con distancia euclídea sobre datos OHE produce grupos distintos a la estructura real — la distancia importa.

---

## Parte 2 — Tarjetas de crédito (datos numéricos, sin etiqueta)

**Notebook:** `workshop-clustering-creditcard.ipynb` · **Dataset:** `data/credit_card.csv`

8.950 clientes con 17 variables numéricas (saldo, compras, adelantos, límite, pagos…). **No hay etiqueta**. El objetivo es segmentar clientes para una estrategia de marketing.

### Qué se trabajó

- EDA: 1 nulo en `CREDIT_LIMIT` y 313 en `MINIMUM_PAYMENTS`, imputados con la **mediana**.
- Observación del sesgo típico en datos financieros.
- **Escalado** con `StandardScaler`.
- **PCA**: 2 componentes explican ~47.6 % de la varianza; 7 componentes superan el 80 %.
- Clustering: **K-Means** (codo + silhouette), **Aglomerativo** (dendrograma), **GMM** y **DBSCAN**.
- Validación sin etiqueta: *silhouette*, *Davies-Bouldin* y *Calinski-Harabasz*.
- Visualización con **t-SNE**.
- Interpretación de perfiles (heatmap de medias por cluster).
- **Isolation Forest** para detectar clientes atípicos.

### Segmentación de clientes (K-Means, k=3 · silhouette 0.251)

| Segmento | Señales de comportamiento | Acción sugerida |
|---|---|---|
| Uso moderado y bajo saldo | Menor saldo, compras y límite; actividad ocasional | Activación, recompensas sencillas |
| Alto consumo y pagos | Mayor volumen de compras, transacciones y pagos | Fidelización, beneficios premium |
| Dependencia de adelantos | Alto saldo, uso elevado del límite, bajo pago completo | Seguimiento de riesgo, planes de pago |

> Los nombres son una interpretación de los perfiles medios, no etiquetas reales.

---

## Tecnologías

Python · Pandas · NumPy · Matplotlib · Seaborn · scikit-learn (`PCA`, `TSNE`, `KMeans`, `AgglomerativeClustering`, `GaussianMixture`, `DBSCAN`, `IsolationForest`, `RandomForestClassifier` y métricas de clustering) · SciPy (`linkage` / `dendrogram`)

---

## Cómo ejecutar

```bash
git clone https://github.com/Bootcamp-IA-MAD-P7/Proyecto-7-Karina.git
cd Proyecto-7-Karina
pip install -r requirements.txt
jupyter notebook
```

Ejecuta los notebooks en este orden:
1. `workshop-clustering-Mushrooms.ipynb`
2. `workshop-clustering-creditcard.ipynb`
