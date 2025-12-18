# Análisis del Sentimiento en Criptomonedas y su Relación con el Tipo de Cambio (USD/PEN)

## 1. Introducción

El objetivo central de este proyecto es evaluar si existe una correlación significativa entre el sentimiento en el mercado de criptomonedas, medido por el **Fear & Greed Index (FGI)**, y el tipo de cambio del sol peruano (**PEN/USD**).

Esta investigación surge de la preocupación por la vulnerabilidad de las economías emergentes ante shocks externos de carácter financiero y de expectativas. La identificación de fuentes adicionales de volatilidad, como el sentimiento en mercados digitales globales, permite a los bancos centrales y analistas financieros mejorar su capacidad para anticipar episodios de depreciación o apreciación cambiaria.

El estudio integra estadística descriptiva, modelos de series de tiempo y técnicas de *machine learning* desarrolladas a lo largo de cuatro etapas progresivas.

---

## 2. Importación y Preparación de Datos

Para el análisis se construyó un dataset consolidado que unifica fuentes del mercado cripto, indicadores financieros globales y series macroeconómicas asociadas al tipo de cambio peruano.

### Proceso de Curaduría

- **Limpieza:** Manejo de inconsistencias originales y corrección de valores faltantes.  
- **Homogeneización:** Estandarización de la frecuencia temporal de todas las series.  
- **Rango Temporal:** El dataset final comprende el periodo desde el **01 de junio de 2018** hasta el **30 de junio de 2025**.

### Variables Seleccionadas

| Variable | Descripción |
|--------|-------------|
| FGI | Fear & Greed Index (Sentimiento del mercado cripto) |
| USD/PEN | Tipo de cambio de venta del sol peruano |
| DXY | Índice del dólar global |
| VIX | Índice de volatilidad (incertidumbre financiera global) |
| BTC/USD | Precio del Bitcoin en dólares |
| Gold | Precio del oro (activo refugio) |
| T-Bills / Treasury | Tasas de interés de bonos del Tesoro de EE.UU. (13w y 5y) |

---

## 3. Análisis Exploratorio (EDA)

El análisis exploratorio permite comprender el comportamiento histórico y la estructura de volatilidad de las variables antes del modelado.

### Evolución Histórica y Comovimientos

- **Tipo de Cambio:**  
  Entre 2018 y 2021, el USD/PEN experimentó una marcada depreciación debido a incertidumbre política doméstica y la pandemia de COVID-19, alcanzando máximos cercanos a **S/ 4.10**.

- **Bitcoin vs. Oro:**  
  Ambos activos muestran una correlación visual positiva desde 2020, respondiendo a la liquidez global, aunque Bitcoin presenta movimientos mucho más erráticos y explosivos.

- **Desconexión Cripto-Cambiaria:**  
  Se observa una desconexión entre el ciclo especulativo de Bitcoin y el sol peruano; mientras el BTC subió en 2024–2025, el sol se mantuvo estable debido a fundamentos locales y la intervención del BCRP.

### Distribuciones de Riesgo

- **USD/PEN:**  
  Presenta una curva estrecha con un pico pronunciado en cero, indicando baja volatilidad estructural y variaciones diarias típicamente inferiores al **0.5%**.

- **Bitcoin:**  
  Exhibe una distribución de *colas pesadas* con variaciones frecuentes de entre **±5%** e incluso superiores al **10%** en una sola sesión, reflejando su naturaleza especulativa.

### Dinámica del Sentimiento (FGI)

El sentimiento del mercado muestra un comportamiento bimodal. Los inversionistas rara vez se encuentran en un estado neutral (solo 400 días); en cambio, el mercado oscila principalmente entre estados de **Miedo** (más de 1200 días) y **Avaricia** (aprox. 950 días).

El análisis de dispersión arroja una correlación de Pearson de solo **0.07** entre el FGI y el tipo de cambio.

---

## 4. Modelo Dinámico ARX

Se implementó un modelo **Autoregresivo con Variable Exógena (ARX)** para evaluar si el sentimiento global aporta información predictiva adicional a la inercia propia del tipo de cambio.

### Especificación del Modelo

El modelo se estimó mediante **Mínimos Cuadrados Ordinarios (OLS)** con errores robustos (**HAC**) para corregir la autocorrelación.

### Resultados Estadísticos

- **Persistencia (Inercia):**  
  El coeficiente del retorno rezagado (*ret_USD_lag1*) es altamente significativo (**p = 0.002**), confirmando que el mercado cambiario peruano tiene memoria a corto plazo.

- **Sentimiento (Exógena):**  
  El coeficiente del FGI rezagado no resultó significativo (**p = 0.623**), lo que implica que las emociones del mercado cripto no explican las variaciones diarias del sol peruano.

- **Capacidad Predictiva:**  
  El modelo ARX mejoró el **Error Cuadrático Medio (MSE)** a **0.092**, comparado con **0.101** del modelo simple, gracias a la inclusión del componente autorregresivo.

En conclusión, el comportamiento del dólar en Perú responde principalmente a su propia inercia y a la intervención estabilizadora local, permaneciendo aislado de la euforia o pánico de los mercados de activos digitales.
