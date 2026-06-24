from pathlib import Path
import nbformat
import re
import zipfile

src = Path("/mnt/data/machine_learning_models.ipynb")
nb = nbformat.read(src, as_version=4)

review_patterns = [
    r"comentario\s+del\s+revisor",
    r"revisor",
    r"reviewer",
    r"correcci[oó]n",
    r"necesita\s+correcci[oó]n",
    r"bien\s+hecho",
    r"muy\s+bien",
    r"hola[!¡\s]*",
    r"felicidades",
    r"buen\s+trabajo",
    r"observaci[oó]n",
    r"sugerencia",
    r"ten\s+en\s+cuenta",
    r"por\s+favor",
    r"tripleten",
    r"practicum",
    r"éxitos",
    r"exitos",
    r"aprobado",
    r"puedes\s+mejorar",
    r"debes\s+",
]

def looks_like_reviewer_cell(text: str) -> bool:
    lower = text.lower()
    hits = sum(bool(re.search(p, lower, flags=re.IGNORECASE)) for p in review_patterns)
    # Reviewer cells are usually markdown/admonitions with conversational feedback.
    if hits >= 1 and ("<div" in lower or "alert" in lower or "revisor" in lower or "reviewer" in lower):
        return True
    if hits >= 2:
        return True
    return False

clean_cells = []
removed = []

for i, cell in enumerate(nb.cells):
    text = cell.get("source", "")
    cell_type = cell.get("cell_type", "")
    
    if looks_like_reviewer_cell(text):
        removed.append((i, cell_type, text[:180].replace("\n", " ")))
        continue
    
    # Remove inline reviewer HTML blocks inside mixed markdown cells
    cleaned = re.sub(
        r"<div[^>]*(?:alert|reviewer|revisor)[\s\S]*?</div>",
        "",
        text,
        flags=re.IGNORECASE
    )
    cleaned = re.sub(
        r"(?is)comentario\s+del\s+revisor[\s\S]*?(?=\n#|\n##|\n```|\Z)",
        "",
        cleaned
    )
    cleaned = cleaned.strip()
    
    if cleaned:
        cell["source"] = cleaned
        clean_cells.append(cell)

nb.cells = clean_cells

# Remove notebook metadata that may contain course/reviewer traces
for key in ["widgets", "varInspector"]:
    nb.metadata.pop(key, None)

out_nb = Path("/mnt/data/machine_learning_models_clean_v2.ipynb")
nbformat.write(nb, out_nb)

# Validate remaining suspicious text
remaining_hits = []
for i, cell in enumerate(nb.cells):
    text = cell.get("source", "")
    lower = text.lower()
    for p in review_patterns:
        if re.search(p, lower, flags=re.IGNORECASE):
            remaining_hits.append((i, cell.get("cell_type", ""), p, text[:160].replace("\n", " ")))
            break

readme = """# Machine Learning Models - Megaline

## Descripción del proyecto

Este proyecto desarrolla un modelo de clasificación para recomendar a los clientes de Megaline el plan de telefonía más adecuado: Smart o Ultra. El análisis parte de datos de comportamiento mensual de usuarios e implementa distintos modelos de Machine Learning para comparar su desempeño.

## Objetivo

Construir, evaluar y seleccionar un modelo capaz de predecir correctamente el plan recomendado para cada usuario, utilizando métricas de clasificación y una prueba final de cordura.

## Datos utilizados

El dataset incluye información agregada sobre el comportamiento de los usuarios, como:

- Número de llamadas realizadas.
- Duración total de llamadas.
- Cantidad de mensajes enviados.
- Consumo de internet.
- Plan utilizado por el cliente.

## Metodología

El proyecto sigue estos pasos:

1. Carga e inspección inicial de los datos.
2. Exploración general del dataset.
3. Separación de variables predictoras y variable objetivo.
4. División de los datos en conjuntos de entrenamiento, validación y prueba.
5. Entrenamiento de modelos de clasificación.
6. Ajuste de hiperparámetros.
7. Comparación del desempeño de los modelos.
8. Evaluación final con el conjunto de prueba.
9. Prueba de cordura mediante un modelo constante.

## Modelos evaluados

- Árbol de decisión.
- Bosque aleatorio.
- Modelo base de regresión lineal usado como referencia académica.

## Métrica principal

La métrica principal utilizada fue `accuracy`, ya que el objetivo del proyecto era medir la proporción de predicciones correctas sobre el total de observaciones evaluadas.

## Principales resultados

El modelo de bosque aleatorio obtuvo el mejor desempeño entre los modelos evaluados. Su capacidad para combinar múltiples árboles permitió mejorar la estabilidad de las predicciones frente a un árbol individual.

## Conclusión

El modelo seleccionado fue Random Forest, ya que presentó el mejor equilibrio entre precisión y capacidad de generalización. Además, superó la prueba de cordura, lo que indica que sus predicciones aportan valor frente a una estrategia básica de predicción constante.

Este proyecto demuestra el flujo completo de un problema supervisado de clasificación: preparación de datos, entrenamiento, validación, comparación de modelos y evaluación final.

## Tecnologías utilizadas

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Jupyter Notebook

## Estructura sugerida del repositorio

```text
machine-learning-models-megaline/
│
├── data/
│   └── users_behavior.csv
├── notebooks/
│   └── machine_learning_models_clean_v2.ipynb
├── README.md
└── requirements.txt
