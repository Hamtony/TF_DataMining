# Predicción temprana de deserción estudiantil en educación superior

**Curso:** Data Mining Tools – CC209 · Universidad Peruana de Ciencias Aplicadas (UPC)

**Docente:** Carlos Fernando Montoya Cubas

**Proyecto Integrador de Data Science:** Trabajo Parcial (TP1) y Trabajo Final (TF1)

**Grupo:** 7

**Integrantes:** :
- Jose Alejandro Loyola Huaman U202110078
- Antonio Francisco Datto Aponte U202017033


---

## 1. Definición del problema

### 1.1 Contexto

La deserción en educación superior afecta a los estudiantes y a sus familias, a las instituciones y a la sociedad en general. Para las instituciones implica pérdida de recursos y de resultados académicos; para los estudiantes, una trayectoria interrumpida con consecuencias económicas y personales. Una parte de los abandonos ocurre en etapas tempranas de la carrera, cuando todavía es posible intervenir con tutorías, apoyo académico o ayudas socioeconómicas.

### 1.2 Necesidad que se desea abordar

Las áreas de acompañamiento estudiantil cuentan con recursos limitados y no pueden intervenir sobre todos los estudiantes. Se necesita una forma de **priorizar a quiénes acompañar primero**, usando información que la institución ya tiene disponible al inicio de la carrera.

### 1.3 Unidad de análisis

Un **estudiante de pregrado** matriculado en una institución de educación superior. Cada fila del dataset corresponde a un estudiante.

### 1.4 Pregunta principal

> **¿Es posible identificar, al finalizar el primer semestre, a los estudiantes que terminarán abandonando su carrera, usando únicamente información de matrícula, contexto socioeconómico y desempeño académico del primer semestre? ¿Qué factores se asocian más con ese abandono?**

Preguntas secundarias:

1. ¿Cuánto mejora un modelo de Machine Learning frente a un baseline trivial (por ejemplo, predecir siempre la clase mayoritaria)?
2. ¿Qué variables (académicas, demográficas, socioeconómicas, macroeconómicas) aportan más a la predicción y son coherentes con el problema?
3. ¿En qué tipo de estudiantes o segmentos el modelo falla más?

### 1.5 Tipo de problema de Data Science

**Clasificación supervisada multiclase** (`Dropout`, `Enrolled`, `Graduate`), con una formulación binaria alternativa (`Dropout` vs. no `Dropout`) que se evaluará durante el TP1.

### 1.6 Punto de predicción (decisión de diseño anti-leakage)

El dataset incluye información de tres momentos: (a) al matricularse, (b) al cierre del 1.er semestre y (c) al cierre del 2.º semestre. Para que el modelo sea útil como **alerta temprana**, el escenario principal usa solo (a) y (b). Las variables del 2.º semestre se excluyen del escenario principal porque, en un uso real, aún no estarían disponibles al momento de decidir a quién acompañar.

### 1.7 Utilidad esperada de la solución

Un listado priorizado de estudiantes en riesgo, con una explicación de los factores que influyen, que permita a un equipo de tutoría decidir dónde intervenir primero.

### 1.8 Criterios para considerar útil el resultado

| Criterio | Descripción |
|---|---|
| Mejora frente al baseline | El modelo supera claramente a `DummyClassifier` en las métricas seleccionadas. |
| Detección de desertores | Se prioriza el **recall de la clase `Dropout`** (un desertor no detectado es el error más costoso), sin perder precisión de forma desproporcionada. |
| Métrica global robusta | Se reporta **F1 macro**, dado el desbalance de clases; la accuracy se reporta solo como referencia. |
| Interpretabilidad | Los factores más influyentes deben ser coherentes con el problema y explicables |
| Reproducibilidad | Otra persona puede reproducir el flujo completo desde este repositorio. |

---

## 2. Dataset

### 2.1 Fuente y procedencia

| Campo | Detalle |
|---|---|
| **Nombre** | Predict Students' Dropout and Academic Success |
| **Repositorio** | UCI Machine Learning Repository, dataset ID 697: <https://archive.ics.uci.edu/dataset/697> 
| **Artículo de referencia** | Realinho, V., Machado, J., Baptista, L., Martins, M. V. (2022). *Predicting Student Dropout and Academic Success*. Data, 7(11), 146. |
| **Origen** | Institución de educación superior de Portugal, integrando varias bases de datos independientes. Proyecto financiado por el programa SATDAP – Capacitação da Administração Pública (Portugal). |
| **Licencia** | Creative Commons Attribution 4.0 International (**CC BY 4.0**): uso y adaptación permitidos con atribución. |

### 2.2 Dimensiones y período

| Característica | Valor |
|---|---|
| Observaciones | **4 424** estudiantes |
| Variables | **36 variables predictoras** + 1 variable objetivo (el artículo original reporta 35 atributos; la diferencia se revisará y documentará en el EDA) |
| Período | Estudiantes matriculados entre los años académicos 2008/2009 y años posteriores (rango exacto a confirmar con el artículo). |
| Formato | CSV con separador `;` |

### 2.3 Variable objetivo

`Target`, con tres categorías según el estado del estudiante al final de la duración normal de la carrera:

| Clase | Significado |
|---|---|
| `Dropout` | Abandonó la carrera |
| `Enrolled` | Sigue matriculado |
| `Graduate` | Se graduó |

### 2.4 Diccionario de variables

| Grupo | Ejemplos de variables | Momento de disponibilidad |
|---|---|---|
| Demográficas y personales | Estado civil, nacionalidad, género, edad al matricularse, desplazado, necesidades educativas especiales | Matrícula |
| Trayectoria académica previa | Modo de postulación, orden de postulación, carrera (`Course`), turno diurno/vespertino, calificación previa, nota de admisión | Matrícula |
| Contexto socioeconómico | Estudios y ocupación de la madre y del padre, beca (`Scholarship holder`), deudor, matrícula al día | Matrícula / año 1 |
| Desempeño 1.er semestre | Unidades curriculares inscritas, evaluadas, aprobadas, promedio, sin evaluaciones | Cierre 1.er semestre |
| Desempeño 2.º semestre | Mismas variables que el 1.er semestre | Cierre 2.º semestre (**excluidas del escenario principal**) |
| Contexto macroeconómico | Tasa de desempleo, tasa de inflación, PIB | Contexto regional |

### 2.5 Justificación de por qué el dataset es adecuado

- Responde directamente a la pregunta: tiene estudiantes y un resultado final observado.
- Su tamaño (4 424 filas) y número de variables permiten hacer EDA, preparación, modelamiento, validación cruzada e interpretabilidad (SHAP) sin requerir infraestructura pesada.
- Mezcla variables numéricas y categóricas, y cuenta con desbalance de clases. Esto exige usar `ColumnTransformer`, elegir métricas apropiadas y justificar el baseline.
- Existen momentos de captura bien diferenciados (matrícula, 1.er y 2.º semestre), lo que permite discutir **data leakage** con un criterio concreto.
- Tiene licencia abierta y documentación del origen.

### 2.6 Limitaciones conocidas del dataset

1. **Contexto geográfico e institucional:** los datos son de una institución portuguesa. Los resultados **no son directamente extrapolables** a universidades peruanas ni a la UPC.
2. **Preprocesamiento previo:** los autores indican que aplicaron limpieza antes de publicarlo; es posible que ya se hayan tomado decisiones que no son visibles.
3. **Clase `Enrolled`:** agrupa a estudiantes cuyo desenlace final aún no se conoce con certeza, lo que la hace conceptualmente ambigua.
4. **Variables codificadas:** la codificación entera de categorías puede inducir a error si no se trata correctamente.
5. **Sin información individual de comportamiento:** no incluye asistencia, uso de plataformas, salud ni motivos declarados de abandono. El modelo puede capturar asociaciones, **no causas**.
6. **Posibles sesgos:** variables como género, nacionalidad o desplazamiento pueden producir tratamiento desigual entre grupos si se usan sin análisis. Se revisará el desempeño por segmentos.

## 3. Instrucciones básicas de ejecución

```bash
# 1. Clonar el repositorio
git clone <URL_DEL_REPOSITORIO>
cd proyecto

# 2. Crear y activar un entorno virtual
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Obtener los datos (opción A: directamente desde UCI)
python -c "from ucimlrepo import fetch_ucirepo; d = fetch_ucirepo(id=697); print(d.data.features.shape)"

# 5. Ejecutar los notebooks en orden (01 → 03)
jupyter lab

---


## 4. Referencias

- Realinho, V., Machado, J., Baptista, L., & Martins, M. V. (2022). Predicting Student Dropout and Academic Success. *Data*, 7(11), 146.
- UCI Machine Learning Repository – Predict Students' Dropout and Academic Success: <https://archive.ics.uci.edu/dataset/697>
- Zenodo (DOI del dataset): <https://doi.org/10.5281/zenodo.5777339>
