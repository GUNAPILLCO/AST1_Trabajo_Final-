## Informe de Análisis de Series Temporales

### a) **Planteamiento de la pregunta de investigación:**

La pregunta central que me motivo a realizar este trabajo fue **¿Cuál es el modelo más adecuado para predecir el precio de cierre del índice E-mini Nasdaq 100 (MNQ) utilizando series temporales minuto a minuto?**

Mi objetivo con este análisis es explorar algunos modelos estadísticos y de machine learning que podrían ser más efectivos para la **predicción del precio de cierre ´close´ del indice miniNASDAQ, un instrumento financiero** en base a datos históricos a alta frecuencia (minuto a minuto). A través de esta pregunta, nos proponemos comparar modelos bien establecidos como **ARIMA** (AutoRegressive Integrated Moving Average), **GARCH** (Generalized Autoregressive Conditional Heteroskedasticity) y **LSTM** (Long Short-Term Memory) para determinar cuál ofrece el mejor desempeño en términos de precisión y fiabilidad en el contexto de precios financieros.

### b) **Descripción de los datos:**
Los datos utilizados provienen de la plataforma **NinjaTrader**, con un enfoque en el contrato de futuros **MNQ 09-24**. El dataset cubre los registros minuto a minuto desde **junio de 2024 hasta septiembre de 2024**, lo que significa que tenemos una gran cantidad de información (450 registros por día), con el objetivo de capturar las fluctuaciones intradía del índice.

El dataset contiene las siguientes variables en formato **OHLCV**:

- **Open**: El precio de apertura de cada intervalo de tiempo.
- **High**: El precio máximo alcanzado dentro de ese minuto.
- **Low**: El precio mínimo alcanzado dentro de ese minuto.
- **Close**: El precio de cierre (variable objetivo para la predicción).
- **Volume**: El volumen de operaciones para ese minuto.

El dataset está inicialmente en la zona horaria **UTC** y fue necesario hacer la conversión de los datos a la zona horaria local para que las fechas y horas fueran coherentes con los análisis realizados.

### c) **Descripción de los modelos:**
Se utilizaron tres modelos diferentes para abordar la predicción del precio de cierre:

1. **ARIMA (AutoRegressive Integrated Moving Average)**: ARIMA es un modelo estadístico clásico utilizado para analizar y pronosticar series temporales. Este modelo se ajusta a los datos históricos de **precio de cierre** (serie `close`) utilizando parámetros **AR** (autoregresivos), **I** (integración) y **MA** (media móvil), con el fin de capturar las dependencias en los datos de acuerdo a sus tendencias y estacionalidades.

2. **GARCH (Generalized Autoregressive Conditional Heteroskedasticity)**: El modelo GARCH se utiliza para modelar la **volatilidad** en los datos de series temporales. En este trabajo, se aplicó a los **retornos logarítmicos** de la serie para capturar los patrones de varianza condicional, es decir, cómo la volatilidad cambia a lo largo del tiempo y cómo influye en los movimientos de los precios.

3. **LSTM (Long Short-Term Memory)**: LSTM es una variante de las redes neuronales recurrentes (RNN) que es particularmente adecuada para el aprendizaje de dependencias a largo plazo en datos secuenciales, como las series temporales. Se utilizó para predecir el **precio de cierre** dentro de una ventana de **180 minutos** previos, prediciendo los **15 minutos siguientes** dentro del mismo día.

### d) **Pruebas sobre los modelos:**
Cada modelo fue evaluado utilizando un conjunto de datos de **entrenamiento** (80%) y **prueba** (20%). Para la evaluación, se emplearon las siguientes métricas de error:

- **MAE (Error Absoluto Medio)**: Medida de la diferencia media absoluta entre las predicciones y los valores reales.
- **RMSE (Raíz del Error Cuadrático Medio)**: Evaluación de la magnitud promedio de los errores en la predicción.
- **MAPE (Error Porcentual Absoluto Medio)**: Mide la precisión de las predicciones en términos porcentuales.
- **R² (Coeficiente de Determinación)**: Indicador que muestra la proporción de la varianza en los datos que el modelo logra predecir.

A continuación, se muestran las métricas obtenidas para cada modelo:

**ARIMA (predicción de precio de cierre):**

- **MAE**: 625.54
- **RMSE**: 779.03
- **MAPE**: 3.26%
- **R²**: -0.44

**GARCH (predicción de volatilidad):**

- **MAE**: 0.00014
- **RMSE**: 0.00019
- **MAPE**: 39.20%
- **R²**: 0.30

**LSTM (predicción de precio de cierre):**

- **MAE**: 0.245
- **RMSE**: 0.304
- **MAPE**: 18.75%
- **R²**: 0.02

**Análisis de resultados**:

- **ARIMA** mostró el **mejor rendimiento** para predecir el **precio de cierre**, ya que sus métricas de error fueron más bajas, a pesar de tener un valor negativo de R², lo que indica que la capacidad explicativa del modelo no fue tan alta, pero aún así realizó predicciones relativamente cercanas a los valores reales.
- **LSTM** presentó un rendimiento más moderado. A pesar de capturar algunas dinámicas a largo plazo, el modelo tuvo un **R² bajo** y una mayor variabilidad en las predicciones. Esto sugiere que podría haber problemas con la capacidad de predicción, aunque el modelo **LSTM** puede tener un potencial de mejora si se ajustan mejor los parámetros y se usan más datos.
- **GARCH**, como modelo para predecir volatilidad, no mostró un buen rendimiento en términos de predicción del precio de cierre. Sin embargo, fue útil para modelar la **volatilidad** en los precios, lo que es relevante para otros análisis financieros.

### e) **Conclusiones**:

- **Lecciones aprendidas**:
  
  - **ARIMA** sigue siendo un modelo robusto para **series temporales con tendencias claras**, ya que es capaz de predecir precios con precisión razonable en este contexto de datos minuto a minuto, aunque no captura algunas dinámicas no lineales.
  - **LSTM**, aunque potente para capturar patrones complejos a largo plazo, necesita más datos o más ajustes en la arquitectura para mejorar su rendimiento.
  - **GARCH** es útil para modelar **volatilidad**, pero no es el modelo adecuado para **predecir precios** directamente. Aún así, puede ser valioso en la **gestión del riesgo** y el **análisis de la volatilidad del mercado**.

- **Problemas y ajustes**:
  
  - Hubo algunos problemas al ajustar los **hiperparámetros** de los modelos y durante la **optimización** de la arquitectura LSTM. Sin embargo, estos son problemas comunes cuando se trabaja con **modelos de deep learning**.
  - **La cantidad de datos** utilizada fue limitada, y es posible que **LSTM** y otros modelos de machine learning puedan mejorar si se entrenan con **más datos** o **más características**.

- **Futuras mejoras**:
  
  - **Mejorar el modelo LSTM**: Considerando más capas, más unidades y más datos para entrenar el modelo.
  - **Probar otros enfoques** de **machine learning** como **Random Forest** o **XGBoost**, que pueden capturar patrones complejos en series temporales sin necesidad de secuencias tan largas.
