## Informe de Análisis de Series Temporales para el índice e-mini NASDAQ minuto a minuto

### a) **Planteamiento de la pregunta de investigación:**

La pregunta central que me motivo a realizar este trabajo fue **¿Existe algún modelo de IA que pueda predecir el movimiento del índice E-mini NASDAQ minuto a minuto?**

Mi objetivo con este análisis es explorar algunos modelos estadísticos y de machine learning que podrían ser más efectivos para la **predicción del precio de cierre ´close´ del indice miniNASDAQ, un instrumento financiero** en base a datos históricos a alta frecuencia (minuto a minuto).

A través de esta pregunta, me propongo comparar tres modelos bien establecidos: el modelo **ARIMA** (AutoRegressive Integrated Moving Average), el modelo **GARCH** (Generalized Autoregressive Conditional Heteroskedasticity) y el modelo **LSTM** (Long Short-Term Memory) para determinar cuál ofrece el mejor desempeño en términos de precisión y fiabilidad en el contexto de precios financieros.

### b) **Descripción de los datos:**

Los datos utilizados provienen de la plataforma **NinjaTrader**, enfocado en el contrato de futuros **MNQ 09-24**. El dataset cubre los registros minuto a minuto desde **junio de 2024 hasta septiembre de 2024**, lo que significa que tenemos una gran cantidad de información (450 registros por día), con el objetivo de capturar las fluctuaciones intradía del índice.

El dataset contiene las siguientes variables en formato **OHLCV**:

- **Open**: El precio de apertura de cada intervalo de tiempo.
- **High**: El precio máximo alcanzado dentro de ese minuto.
- **Low**: El precio mínimo alcanzado dentro de ese minuto.
- **Close**: El precio de cierre (variable objetivo para la predicción).
- **Volume**: El volumen de operaciones para ese minuto.

El dataset está inicialmente en la zona horaria **UTC** y fue necesario hacer la conversión de los datos a la zona horaria local (Nueva York) para que las fechas y horas fueran coherentes con los análisis realizados.

### c) **Descripción de los modelos:**

Como mencioné antes, utilicé tres modelos diferentes para abordar la predicción del precio de cierre:

1. **ARIMA (AutoRegressive Integrated Moving Average)**: ARIMA es un modelo estadístico clásico utilizado para analizar y pronosticar series temporales. Este modelo se ajusta a los datos históricos de **precio de cierre** (serie `close`) utilizando parámetros **AR** (autoregresivos), **I** (integración) y **MA** (media móvil), con el fin de capturar las dependencias en los datos de acuerdo a sus tendencias y estacionalidades.

2. **GARCH (Generalized Autoregressive Conditional Heteroskedasticity)**: El modelo GARCH se utiliza para modelar la **volatilidad** en los datos de series temporales. En este trabajo, se aplicó a los **retornos logarítmicos** de la serie `close` para capturar los patrones de varianza condicional, es decir, cómo la volatilidad cambia a lo largo del tiempo y cómo influye en los movimientos de los precios.

3. **LSTM (Long Short-Term Memory)**: LSTM es una variante de las redes neuronales recurrentes (RNN) que es particularmente adecuada para el aprendizaje de dependencias a largo plazo en datos secuenciales, como las series temporales. Se utilizó para predecir el **precio de cierre** dentro de una ventana de **180 minutos** previos, prediciendo los **15 minutos siguientes** dentro del mismo día.

### d) **Pruebas sobre los modelos:**
Cada modelo fue evaluado utilizando un conjunto de datos de **entrenamiento** (80%) y **prueba** (20%). Para la evaluación, se emplearon las siguientes métricas de error:

- **MAE (Error Absoluto Medio)**: Medida de la diferencia media absoluta entre las predicciones y los valores reales.
- **RMSE (Raíz del Error Cuadrático Medio)**: Evaluación de la magnitud promedio de los errores en la predicción.
- **MAPE (Error Porcentual Absoluto Medio)**: Mide la precisión de las predicciones en términos porcentuales.
- **R² (Coeficiente de Determinación)**: Indicador que muestra la proporción de la varianza en los datos que el modelo logra predecir.

A continuación, se muestran las predicciones y las métricas obtenidas para cada modelo:

**ARIMA (predicción de close):**

![image](https://github.com/user-attachments/assets/48fece28-2edf-41f2-8591-5a7834836c6d)

- **MAE**: 625.54
- **RMSE**: 779.03
- **MAPE**: 3.26%
- **R²**: -0.44

**GARCH (predicción de volatilidad de close):**

![image](https://github.com/user-attachments/assets/1a33f550-5587-432b-b134-97b5a2ad8de2)

- **MAE**: 0.00014
- **RMSE**: 0.00019
- **MAPE**: 39.20%
- **R²**: 0.30

**LSTM (predicción de close):**
![image](https://github.com/user-attachments/assets/51d63a07-aac0-4ca2-a922-39fa67de8672)

- **MAE**: 23.90
- **RMSE**: 30.0219
- **MAPE**: 0.0010
- **R²**: -0.0177 

## Comparación de modelos de predicción

### Tabla comparativa de métricas

| **Métrica** | **ARIMA (close)** | **GARCH (volatilidad)** | **LSTM (close)** | **Interpretación** |
|------------|-------------------|--------------------------|------------------|---------------------|
| **MAE**    | 625.54            | 0.00014                  | 23.90            | LSTM logra el menor error absoluto para `close`. GARCH tiene valores pequeños por escala. |
| **RMSE**   | 779.03            | 0.00019                  | 30.02            | LSTM muestra menor dispersión del error cuadrático que ARIMA. |
| **MAPE**   | 3.26%             | 39.20%                   | 0.0010%          | LSTM supera ampliamente en términos relativos. GARCH tiene MAPE elevado por su escala y variabilidad. |
| **R²**     | -0.44             | 0.30                     | -0.0177          | Solo GARCH presenta capacidad explicativa positiva. ARIMA y LSTM tienen desempeño inferior al promedio. |

---

## Análisis de resultados

- **LSTM** mostró el **mejor rendimiento global** en la predicción del **precio de cierre**, con los **menores errores absolutos (MAE y RMSE)** y un **MAPE extremadamente bajo**, lo que indica una alta precisión relativa. Sin embargo, su **R² negativo** sugiere que aún no logra explicar adecuadamente la variabilidad total de la serie, posiblemente por subajuste o arquitectura no óptima.

- **ARIMA**, a pesar de ser un modelo clásico, tuvo errores considerablemente mayores en comparación con LSTM. El **MAPE moderado** indica cierto nivel de acierto en proporción al precio, pero el **R² muy negativo** confirma que el modelo no logra capturar adecuadamente la dinámica del mercado.

- **GARCH**, al estar orientado a la **predicción de la volatilidad** y no del precio, presentó **valores de error absolutos bajos** (por la escala de los log-retornos), pero un **MAPE elevado**. Aun así, fue el único modelo con **R² positivo**, lo que indica que logró capturar parcialmente la estructura de la volatilidad.

---

## Conclusiones

### Lecciones aprendidas

- **LSTM** demostró ser prometedor para predecir precios en series complejas, aunque su potencial dependerá de una mejor optimización y mayor volumen de datos.

- **ARIMA** ofrece una referencia útil, pero queda limitado ante dinámicas no lineales o cambios estructurales frecuentes.

- **GARCH** cumple su función en el modelado de **volatilidad**, siendo valioso para análisis de **riesgo** más que para predicción de precios.

### Problemas y ajustes

- El **ajuste de hiperparámetros** y la arquitectura del LSTM podrían no haber sido los óptimos.

- La **cantidad limitada de datos** y la posible falta de **características adicionales** (como indicadores técnicos) podrían haber afectado el rendimiento del modelo neuronal.

### Futuras mejoras

- Explorar arquitecturas LSTM más profundas y con mayor regularización.

- Incorporar más datos históricos y variables exógenas (técnicas, fundamentales, etc.).

- Evaluar modelos alternativos como **XGBoost** o **Redes Neuronales Convolucionales (CNN)** aplicadas a series temporales.

