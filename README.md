# Análisis del Sentimiento en Criptomonedas y su Relación con el Tipo de Cambio (USD/PEN)

**Curso:** Introducción a Machine Learning con Python  
**Grupo:** N.° 8  

**Integrantes:**
- Luis Ángel Alejandro Arrieta Feria  
- Mirelli Thais Jimenez Pulache  
- Néstor Julio Rivero Escobar  

---

## 1. Introducción

El objetivo central de este proyecto es evaluar si existe una correlación significativa entre el sentimiento en el mercado de criptomonedas, medido por el **Fear & Greed Index (FGI)**, y el tipo de cambio del sol peruano (**USD/PEN**).

La investigación busca determinar si factores financieros *no tradicionales* y expectativas globales en mercados digitales pueden servir como indicadores de volatilidad para analistas y hacedores de política monetaria.

---

## TRABAJO 1: Importación, Preparación y Análisis Exploratorio (EDA)

## 2. Preparación del Dataset

Se construyó un dataset consolidado que integra series de tiempo de diversas fuentes, asegurando la homogeneidad temporal y la corrección de inconsistencias originales. El dataset final abarca un rango de fechas desde el **01 de junio de 2018** hasta el **30 de junio de 2025**.

### Variables seleccionadas para el estudio

- **FGI (Fear & Greed Index):** Sentimiento del mercado cripto  
- **USD/PEN (Venta):** Tipo de cambio local  
- **DXY:** Índice del dólar global  
- **VIX:** Índice de volatilidad y termómetro del miedo global  
- **BTC/USD:** Precio del Bitcoin  
- **Commodities y Tasas:** Precio del oro, T-Bills 13w y Treasury 5y  

---

## 3. Análisis Exploratorio de Datos (EDA)

El análisis descriptivo permite identificar patrones de estabilidad, episodios de volatilidad y posibles comovimientos entre activos.

### 3.1 Evolución Histórica y Estabilidad

- **USD/PEN:**  
  El sol peruano mostró una marcada depreciación entre 2018 y 2021 debido a la incertidumbre política interna y el impacto del COVID-19, alcanzando picos cercanos a **S/ 4.10**. Posteriormente, ingresó en una fase de estabilización gracias a la intervención del Banco Central de Reserva del Perú (BCRP).

- **Bitcoin vs. Oro:**  
  Ambos activos muestran una correlación visual positiva desde 2020, actuando como reservas de valor ante la liquidez global. Sin embargo, el Bitcoin exhibe movimientos mucho más erráticos y explosivos en comparación con la trayectoria suave del oro.

### 3.2 Distribuciones de Riesgo y Volatilidad

- **USD/PEN:**  
  Sus retornos diarios presentan una curva estrecha con un pico pronunciado en cero, lo que refleja una baja volatilidad estructural y un mercado cambiario predecible sujeto a un marco institucional sólido.

- **Bitcoin:**  
  Posee una distribución de retornos mucho más ancha con *colas gordas* (*fat tails*). Sus movimientos diarios suelen oscilar entre **±5%**, evidenciando su naturaleza especulativa y la ausencia de una autoridad que estabilice su demanda.

### 3.3 Dinámica del Sentimiento (FGI) y Correlaciones

- **Bimodalidad del Sentimiento:**  
  El FGI no sigue una distribución uniforme; se observan dos picos en los extremos (*Miedo* y *Avaricia*). El mercado pasa más tiempo en estados de *Fear* y *Extreme Fear* (más de 1200 días acumulados) que en estados de neutralidad.

- **Validación de Hipótesis:**  
  La correlación de Pearson entre el FGI y el tipo de cambio USD/PEN es de **0.07**, confirmando la ausencia de una relación lineal significativa entre el sentimiento cripto y la moneda peruana.  
  Por el contrario, Bitcoin muestra una correlación moderada de **0.52** con el USD/PEN, influenciada principalmente por el periodo de coincidencia alcista de 2020–2021.

---

## TRABAJO 2: Modelo Dinámico ARX

## 4. Modelo Autoregresivo con Variable Exógena (ARX)

Se implementó un modelo dinámico para evaluar si el retorno del tipo de cambio (\(ret\_USD\)) puede ser explicado por su propia inercia y por el sentimiento global (FGI) como shock externo.

### Especificación del Modelo

\[
ret\_USD_t = \alpha + \beta_1 \cdot ret\_USD_{t-1} + \beta_2 \cdot FGI_{t-1} + \epsilon_t
\]

### 4.1 Resultados del Modelado

El modelo fue ajustado utilizando **Mínimos Cuadrados Ordinarios (OLS)** con errores robustos (**HAC**) para corregir la autocorrelación.

- **Inercia del Mercado (Memoria):**  
  El coeficiente de \(ret\_USD_{t-1}\) resultó positivo y altamente significativo (**p = 0.002**). Esto indica que el tipo de cambio peruano presenta persistencia: si el dólar sube hoy, existe una probabilidad estadística de que mantenga esa tendencia al día siguiente.

- **Influencia del Sentimiento:**  
  El coeficiente del \(FGI_{t-1}\) no fue estadísticamente significativo (**p = 0.623**). Esto refuerza la conclusión de que las condiciones de miedo o avaricia en el mercado cripto no tienen un efecto inmediato sobre la variación diaria del sol peruano.

- **Métrica de Error:**  
  El modelo alcanzó un **Error Cuadrático Medio (MSE)** de **0.092062** en el conjunto de prueba, superando el desempeño de modelos lineales simples previos.

