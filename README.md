# Machine Learning para Recomendación de Planes de Telecomunicaciones

## Descripción del proyecto

La empresa Megaline busca desarrollar un sistema capaz de recomendar automáticamente uno de sus planes telefónicos (Smart o Ultra) a partir del comportamiento de sus usuarios.

En este proyecto se construyeron y evaluaron diferentes modelos de Machine Learning supervisado con el objetivo de identificar la alternativa con mejor capacidad predictiva y utilizarla para apoyar futuras estrategias comerciales.

## Objetivo

Desarrollar un modelo de clasificación que permita predecir correctamente qué plan debería recomendarse a un usuario según sus patrones de uso de servicios de telecomunicaciones.

## Datos

El conjunto de datos contiene información agregada sobre el comportamiento mensual de los clientes, incluyendo:

- Número de llamadas realizadas.
- Minutos consumidos.
- Cantidad de mensajes enviados.
- Consumo de internet.
- Plan utilizado por el cliente.

La variable objetivo corresponde al plan contratado por cada usuario.

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Scikit-Learn
- Matplotlib
- Jupyter Notebook

## Metodología

### 1. Preparación de los datos

- Exploración inicial del dataset.
- Verificación de tipos de datos.
- Identificación de posibles inconsistencias.
- Separación de variables predictoras y variable objetivo.

### 2. División de los datos

El conjunto de datos fue dividido en:

- Entrenamiento
- Validación
- Prueba

Esta estrategia permitió evaluar la capacidad de generalización de cada modelo antes de realizar la evaluación final.

### 3. Entrenamiento de modelos

Se entrenaron y compararon los siguientes algoritmos:

- Decision Tree Classifier
- Random Forest Classifier
- Modelo base de referencia.

Para los modelos basados en árboles se realizó ajuste de hiperparámetros con el fin de maximizar el rendimiento.

### 4. Evaluación

La métrica principal utilizada fue:

**Accuracy**

Esta métrica mide la proporción de predicciones correctas realizadas por el modelo sobre el total de observaciones evaluadas.

Además, se realizó una prueba de cordura para verificar que el modelo seleccionado aportara valor frente a una estrategia trivial.

## Resultados

Tras comparar los modelos entrenados, Random Forest obtuvo el mejor desempeño en los conjuntos de validación y prueba.

Las principales ventajas observadas fueron:

- Mayor precisión predictiva.
- Menor sensibilidad al sobreajuste.
- Mejor capacidad de generalización frente a nuevos datos.

## Conclusiones

- Fue posible construir un modelo capaz de recomendar planes de manera automática utilizando datos históricos de comportamiento de usuarios.
- Los algoritmos basados en árboles superaron al modelo base utilizado como referencia.
- Random Forest presentó el mejor desempeño general y fue seleccionado como modelo final.
- La prueba de cordura confirmó que el modelo genera predicciones significativamente mejores que una estrategia simple.

## Habilidades demostradas

- Análisis exploratorio de datos.
- Preparación y limpieza de datos.
- División de datasets para entrenamiento y validación.
- Entrenamiento de modelos de clasificación.
- Ajuste de hiperparámetros.
- Evaluación de modelos mediante métricas de desempeño.
- Selección de modelos basada en evidencia cuantitativa.
- Comunicación de resultados de Machine Learning.

## Estructura del repositorio

```text
├── machine_learning_models.ipynb
├── README.md
├── data/
│   └── users_behavior.csv
└── images/
```

## Autor

**Alejandro Cotes**  
Estudiante de Ciencia de Datos | Machine Learning | Analítica de Datos
