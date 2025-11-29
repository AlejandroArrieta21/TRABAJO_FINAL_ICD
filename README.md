# TRABAJO_FINAL_ICD
El objetivo del trabajo es evaluar si existe una correlación significativa entre el sentimiento en el mercado de criptomonedas (medido por el Fear &amp; Greed Index) y el tipo de cambio de monedas emergentes como el sol peruano (PEN/USD).

Proyecto Final: Análisis del Sentimiento en Criptomonedas y su Relación con el Tipo de Cambio

Curso: Introducción a Machine Learning con Python
Grupo: N° 8

Integrantes:

Luis Ángel Alejandro Arrieta Feria

Mirelli Thais Jimenez Pulache

Néstor Julio Rivero Escobar

Tema del Proyecto

¿Existe una correlación significativa entre el sentimiento en el mercado de criptomonedas (medido por el Fear & Greed Index) y el tipo de cambio USD/PEN?

1. Introducción

El objetivo central de este proyecto es evaluar si existe una relación significativa entre la evolución del sentimiento en el mercado de criptomonedas, medido mediante el Fear & Greed Index (FGI), y el comportamiento del tipo de cambio USD/PEN, correspondiente a una economía emergente como la peruana.

La motivación surge de la creciente interconexión entre los mercados financieros globales. Aunque tradicionalmente el tipo de cambio responde a factores macroeconómicos, es plausible que indicadores alternativos —como el sentimiento global de riesgo— afecten indirectamente a economías emergentes, especialmente en contextos de alta volatilidad internacional.

Para contrastar esta hipótesis, se construyó un dataset propio, consolidando series del mercado cripto, indicadores financieros globales y el tipo de cambio peruano. A lo largo de los Trabajos 1–4, se integraron métodos de estadística descriptiva, modelos de series de tiempo y herramientas modernas de machine learning.

TRABAJO 1 – Principales Resultados
2. Importación y Preparación de Datos

En el Trabajo 1 se realizó el preprocesamiento completo del dataset: limpieza de valores faltantes, homogeneización temporal, estandarización de nombres y unión de las distintas fuentes. En esta fase solo se carga el dataset final, que contiene las variables clave utilizadas en los modelos:

FGI (Fear & Greed Index)

USD/PEN (tipo de cambio, venta)

DXY (índice del dólar)

VIX (índice de volatilidad global)

BTC/USD (precio de Bitcoin)

Gold (precio del oro)

Treasury Bills 13w

Treasury 5y

El propósito es garantizar que los datos están listos para:

Modelos ARX

Análisis exploratorio

Modelos de regresión y ML

Evaluación causal mediante DAGs

Estas variables resumen factores globales de riesgo, liquidez, volatilidad y sentimiento que potencialmente afectan la estabilidad cambiaria peruana.

3. Análisis Exploratorio (EDA)
3.1. Evolución Histórica de las Principales Variables

Se graficaron las series históricas del tipo de cambio, el FGI, Bitcoin, Oro y VIX.
Los gráficos permiten observar:

Tendencia suave y persistente del USD/PEN.

Alta volatilidad del BTC/USD.

Periodos de “miedo extremo” en el FGI.

Episodios de volatilidad global donde el VIX supera valores críticos (30+).

Esta inspección visual inicial revela que los shocks globales afectan simultáneamente a varios activos, pero no anticipa una relación obvia entre FGI y USD/PEN.

3.2. Histogramas de Retornos

Se calcularon retornos diarios de USD/PEN y Bitcoin.

El USD/PEN presenta retornos muy concentrados cerca de 0 → alta estabilidad.

Bitcoin muestra una distribución mucho más dispersa y con colas pesadas → alto riesgo.

Este análisis destaca la naturaleza distinta de ambos mercados.

3.3. Gráficos de Dispersión

Se analizaron las relaciones:

FGI vs. ret_USD

BTC/USD vs. ret_USD

Resultados descriptivos:

No se observa una relación lineal evidente con FGI.

Bitcoin sí muestra cierta asociación, pero no muy intensa.

Las correlaciones simples son bajas.

Esto anticipa que, si existe relación causal, sería débil o estaría mediada por otros factores.

3.4. Mapa de Calor de Correlaciones

La matriz de correlaciones muestra:

Baja correlación entre FGI y USD/PEN.

Alta correlación entre activos de refugio (oro) y variables globales (VIX, DXY).

Bitcoin correlaciona moderadamente con indicadores de riesgo.

El heatmap permite descartar redundancias y guiar la selección de variables para modelos futuros.

3.5. Bimodalidad del Fear & Greed Index

Se clasificó el FGI en cinco categorías:

Extreme Fear

Fear

Neutral

Greed

Extreme Greed

La distribución muestra predominancia de estados moderados, con eventos puntuales de miedo extremo. Esto refuerza la hipótesis de asimetrías en periodos de tensión financiera.

TRABAJO 2 – Principales Resultados
4. Modelo Dinámico ARX

Se estimó un modelo ARX(1) donde:

𝑟
𝑒
𝑡
_
𝑈
𝑆
𝐷
𝑡
=
𝛼
+
𝛽
 
𝑟
𝑒
𝑡
_
𝑈
𝑆
𝐷
𝑡
−
1
+
𝛾
 
𝐹
𝐺
𝐼
𝑡
−
1
+
𝑢
𝑡
ret_USD
t
	​

=α+βret_USD
t−1
	​

+γFGI
t−1
	​

+u
t
	​


Objetivos del ARX:

Identificar memoria (inercia) en el tipo de cambio.

Evaluar si el FGI aporta información predictiva.

Comparar su MSE con modelos previos.

Resultados:

El coeficiente autoregresivo es significativo → el retorno del tipo de cambio tiene fuerte inercia.

El rezago del FGI no es significativo → el sentimiento cripto no afecta los retornos cambiarios.

El MSE del ARX mejora marginalmente respecto al AR puro, pero el aporte del FGI es mínimo.

TRABAJO 3 – Principales Resultados
5. Modelos Avanzados de Regresión

Se implementaron:

Ridge Regression (L2)

Random Forest

XGBoost

MLPRegressor (Neural Network)

5.1. Feature Engineering

Se generaron rezagos:

t−1

t−2

t−7

de:

USD/PEN

BTC/USD

FGI

Variables macro

El dataset final excluye niveles para evitar leakage.

5.2. TimeSeriesSplit

Se utilizó validación cruzada para series temporales con 5 divisiones y un test final del 10 %.

5.3. Modelo Ridge

Requiere estandarización previa.

Selección óptima de alpha mediante RidgeCV.

MSE moderado: rinde bien pero es lineal → limita su capacidad predictiva.

5.4. Random Forest

Captura no linealidades e interacciones.

Mejor desempeño que Ridge.

Muestra que rezagos y BTC son relevantes, pero no el FGI.

5.5. XGBoost

Mejor desempeño general.

Captura patrones más complejos.

MSE más bajo entre todos los modelos probados.

5.6. Comparación de MSE

XGBoost > Random Forest > Ridge

MLP queda por debajo de todos los modelos.

5.7. Importancia de Variables

El Random Forest muestra que:

Los rezagos de USD/PEN son dominantes.

Bitcoin tiene importancia moderada.

El FGI aparece entre las menos relevantes.

5.8. Tendencias Acumuladas

Las predicciones suavizan la tendencia.
Los mejores modelos siguen la dirección general del tipo de cambio, pero no capturan todos los shocks.

Enfoque Opcional: Two-Stage Model
5.9. Separación entre Inercia y Shocks Exógenos

Etapa 1: AR(1) captura la dinámica interna.

Etapa 2: RF predice los residuos usando variables externas.

Resultados:

Los shocks exógenos sí existen, pero no son explicados por FGI.

5.10. Importancia en los Shocks

Variables globales como VIX, DXY y BTC explican mejor los shocks.
FGI nuevamente tiene baja contribución.

5.11. Gráfico Final de Componentes

Muestra:

Retorno total

Shock real

Shock predicho

El modelo reproduce parcialmente los shocks externos.

TRABAJO 4 – Análisis Causal
6. DAG (Directed Acyclic Graph)

Se elaboró un DAG que incluye:

Variables observadas:

FGI

Bitcoin

USD/PEN

VIX

DXY

Oro

T-Bills

Treasury 5y

Variables no observadas:

Política monetaria

Flujos de capital

Shocks globales

El DAG representa:

Relaciones directas entre sentimiento, activos globales y tipo de cambio.

Confounders como política monetaria que afectan varias rutas y deben controlarse.

Dinámica AR del tipo de cambio.

Permite identificar rutas causales abiertas/cerradas y posibles sesgos de omisión.

7. MLP (Redes Neuronales)

Se implementó un MLPRegressor con arquitectura 64–32, ReLU, adam y early stopping.

Conclusión:

MSE alto y R² negativo.

No logra capturar la estructura temporal.

Requiere mayor cantidad de datos para ser competitivo.

XGBoost y Random Forest superan ampliamente al MLP.

8. Conclusiones Generales

El FGI NO tiene un efecto significativo sobre los retornos del USD/PEN.

El tipo de cambio peruano depende principalmente de:

su propia inercia,

shocks globales amplios,

factores macroestructurales.

Bitcoin sí muestra relación con el USD/PEN, pero su aporte predictivo marginal es pequeño.

XGBoost y Random Forest obtienen los mejores resultados predictivos.

El MLP no generaliza debido al tamaño y naturaleza de los datos.

La evidencia respalda que el mercado cambiario peruano es estable y poco sensible a indicadores de sentimiento cripto.

9. Discusión Económica

Estos hallazgos coinciden con la literatura empírica:

El sentimiento afecta principalmente a activos especulativos, no a monedas estables (Baker & Wurgler, 2007).

Bitcoin correlaciona con el apetito global por riesgo, pero no constituye un determinante estructural de tipos de cambio (Baur et al., 2018; Corbet et al.).

En economías emergentes con políticas creíbles (como Perú), los shocks externos relevantes provienen de variables globales amplias: VIX, DXY, tasas del Tesoro, precios de commodities.

En conjunto, los resultados indican que el tipo de cambio peruano responde más a fundamentos macroeconómicos robustos y a shocks financieros globales que a indicadores de sentimiento propios del criptomercado.
