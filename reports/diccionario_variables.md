# Diccionario de variables

## Fuente y alcance

Este diccionario se basa en la ficha y los metadatos oficiales de UCI para [Predict Students' Dropout and Academic Success](https://archive.ics.uci.edu/dataset/697/predict+students+dropout+and+academic+success). La fuente describe 4.424 estudiantes de una institución de educación superior de Portugal. Cada fila representa a un estudiante.

UCI informa **36 features** y un objetivo separado. El CSV del proyecto contiene **37 columnas**: esos 36 predictores y `Target`. Los nombres que aparecen en la primera columna coinciden con los encabezados del CSV después de quitar los espacios periféricos.

La disponibilidad temporal es relevante para el modelamiento: el escenario de alerta temprana definido en el README utiliza información disponible al matricularse y al cierre del primer semestre. Las variables del segundo semestre son posteriores al punto de decisión y deben excluirse del escenario principal para evitar *data leakage*.

## Variables de matrícula y contexto

| Variable en el CSV | Tipo según UCI | Descripción y codificación | Disponibilidad |
|---|---|---|---|
| `Marital status` | Entero, categórica | Estado civil: 1 soltero/a; 2 casado/a; 3 viudo/a; 4 divorciado/a; 5 unión de hecho; 6 separación legal. | Matrícula |
| `Application mode` | Entero, categórica | Modalidad administrativa de postulación/admisión. Los códigos identifican vías como contingente general, fases especiales, estudiante internacional, mayores de 23 años, traslados y cambios de carrera o institución. Consultar el codebook UCI para el significado de cada código. | Matrícula |
| `Application order` | Entero, ordinal | Orden de preferencia de la postulación: 0 corresponde a primera opción y 9 a última opción. | Matrícula |
| `Course` | Entero, categórica | Código del programa/carrera. Incluye, entre otros, Agronomía (9003), Diseño de Comunicación (9070), Enfermería (9500), Turismo (9254), Periodismo y Comunicación (9773), Educación Básica (9853), Gestión (9147) y Servicio Social (9238); UCI publica el mapeo completo. | Matrícula |
| `Daytime/evening attendance` | Entero, binaria | Turno: 1 diurno; 0 vespertino. | Matrícula |
| `Previous qualification` | Entero, categórica | Nivel/tipo de estudios previos (por ejemplo, educación secundaria, estudios superiores, curso técnico o ciclo de educación básica). Los códigos no representan una escala lineal; ver el codebook UCI para todas las categorías. | Matrícula |
| `Previous qualification (grade)` | Continua | Calificación de la cualificación previa, entre 0 y 200 según UCI. | Matrícula |
| `Nacionality` | Entero, categórica | Nacionalidad codificada (por ejemplo, 1 portuguesa, 41 brasileña, 62 rumana); UCI incluye el mapeo de todos los códigos observados. El nombre `Nacionality` conserva la grafía del CSV. | Matrícula |
| `Mother's qualification` | Entero, categórica | Nivel educativo de la madre, codificado en categorías de educación escolar, técnica y superior. Incluye categorías explícitas como “desconocido” (código 34) y “no sabe leer ni escribir” (35); no equivalen a valores faltantes del CSV. | Contexto familiar registrado al matricularse |
| `Father's qualification` | Entero, categórica | Nivel educativo del padre, codificado en categorías de educación escolar, técnica y superior. Incluye categorías explícitas como “desconocido” (código 34); no equivale a un valor faltante. | Contexto familiar registrado al matricularse |
| `Mother's occupation` | Entero, categórica | Grupo/código de ocupación de la madre: categorías generales y códigos ocupacionales más específicos; incluye estudiante (0), otras situaciones (90) y código 99 descrito por UCI como vacío. Es una categoría codificada, no una magnitud. | Contexto familiar registrado al matricularse |
| `Father's occupation` | Entero, categórica | Grupo/código de ocupación del padre: categorías generales y códigos ocupacionales más específicos; incluye estudiante (0), otras situaciones (90) y código 99 descrito por UCI como vacío. Es una categoría codificada, no una magnitud. | Contexto familiar registrado al matricularse |
| `Admission grade` | Continua | Calificación de admisión, entre 0 y 200 según UCI. | Matrícula |
| `Displaced` | Entero, binaria | Estudiante desplazado/a: 1 sí; 0 no. | Matrícula |
| `Educational special needs` | Entero, binaria | Necesidades educativas especiales: 1 sí; 0 no. | Matrícula |
| `Debtor` | Entero, binaria | Deudor/a: 1 sí; 0 no. | Matrícula / información financiera del año 1 |
| `Tuition fees up to date` | Entero, binaria | Pagos de matrícula al día: 1 sí; 0 no. | Matrícula / información financiera del año 1 |
| `Gender` | Entero, binaria | Género según la codificación UCI: 1 masculino; 0 femenino. | Matrícula |
| `Scholarship holder` | Entero, binaria | Beneficiario/a de beca: 1 sí; 0 no. | Matrícula |
| `Age at enrollment` | Entero | Edad del estudiante al matricularse. | Matrícula |
| `International` | Entero, binaria | Estudiante internacional: 1 sí; 0 no. | Matrícula |

## Desempeño al cierre del primer semestre

Los conteos representan unidades curriculares (asignaturas) del primer semestre. La nota promedio está en una escala de 0 a 20 según UCI.

| Variable en el CSV | Tipo según UCI | Descripción | Disponibilidad |
|---|---|---|---|
| `Curricular units 1st sem (credited)` | Entero | Número de unidades curriculares reconocidas/convalidadas. | Cierre del 1.er semestre |
| `Curricular units 1st sem (enrolled)` | Entero | Número de unidades curriculares en las que se matriculó. | Cierre del 1.er semestre |
| `Curricular units 1st sem (evaluations)` | Entero | Número de evaluaciones realizadas de unidades curriculares. | Cierre del 1.er semestre |
| `Curricular units 1st sem (approved)` | Entero | Número de unidades curriculares aprobadas. | Cierre del 1.er semestre |
| `Curricular units 1st sem (grade)` | Numérico | Nota promedio del semestre, entre 0 y 20. Aunque UCI etiqueta el campo como entero, el CSV puede contener promedios decimales. | Cierre del 1.er semestre |
| `Curricular units 1st sem (without evaluations)` | Entero | Número de unidades curriculares sin evaluaciones. | Cierre del 1.er semestre |

## Desempeño al cierre del segundo semestre

Estos campos describen las mismas medidas para el segundo semestre. Se incluyen en el diccionario para documentar el dataset completo, pero **no deben utilizarse como predictores de la alerta definida al cierre del primer semestre**.

| Variable en el CSV | Tipo según UCI | Descripción | Disponibilidad |
|---|---|---|---|
| `Curricular units 2nd sem (credited)` | Entero | Número de unidades curriculares reconocidas/convalidadas. | Cierre del 2.º semestre; excluir del escenario temprano |
| `Curricular units 2nd sem (enrolled)` | Entero | Número de unidades curriculares en las que se matriculó. | Cierre del 2.º semestre; excluir del escenario temprano |
| `Curricular units 2nd sem (evaluations)` | Entero | Número de evaluaciones realizadas de unidades curriculares. | Cierre del 2.º semestre; excluir del escenario temprano |
| `Curricular units 2nd sem (approved)` | Entero | Número de unidades curriculares aprobadas. | Cierre del 2.º semestre; excluir del escenario temprano |
| `Curricular units 2nd sem (grade)` | Numérico | Nota promedio del semestre, entre 0 y 20. El CSV puede contener promedios decimales. | Cierre del 2.º semestre; excluir del escenario temprano |
| `Curricular units 2nd sem (without evaluations)` | Entero | Número de unidades curriculares sin evaluaciones. | Cierre del 2.º semestre; excluir del escenario temprano |

## Contexto macroeconómico

| Variable en el CSV | Tipo según UCI | Descripción | Disponibilidad |
|---|---|---|---|
| `Unemployment rate` | Continua | Tasa de desempleo, expresada como porcentaje (%). | Contexto macroeconómico asociado al registro; confirmar su fecha de referencia antes de un uso operativo |
| `Inflation rate` | Continua | Tasa de inflación, expresada como porcentaje (%). | Contexto macroeconómico asociado al registro; confirmar su fecha de referencia antes de un uso operativo |
| `GDP` | Continua | Producto interno bruto (PIB). La ficha UCI no especifica unidades en el diccionario. | Contexto macroeconómico asociado al registro; confirmar su fecha de referencia antes de un uso operativo |

## Variable objetivo

| Variable en el CSV | Tipo según UCI | Descripción | Uso |
|---|---|---|---|
| `Target` | Categórica | Resultado del estudiante al final de la duración normal del programa: `Dropout` (abandonó), `Enrolled` (continúa matriculado) o `Graduate` (se graduó). | Etiqueta supervisada; no es predictor |

## Notas de interpretación

- Los códigos de carrera, modalidad de postulación, nacionalidad, cualificaciones y ocupaciones son identificadores de categorías. No asumir que una diferencia numérica entre códigos representa una diferencia de nivel.
- Las categorías con significado similar a “desconocido” o “vacío” forman parte de las codificaciones documentadas por UCI; distinguirlas de valores nulos. UCI declara que el dataset no contiene valores faltantes.
- El codebook UCI contiene el detalle completo de los códigos de modalidad, carrera, nacionalidad, cualificaciones y ocupaciones: [tabla oficial de variables](https://archive.ics.uci.edu/dataset/697/predict+students+dropout+and+academic+success). La API de metadatos está disponible en [UCI API, dataset 697](https://archive.ics.uci.edu/api/dataset?id=697).
- El dataset procede de una institución portuguesa. Sus asociaciones no deben interpretarse como causales ni extrapolarse automáticamente a UPC o al sistema universitario peruano.

## Referencias

- Realinho, V., Vieira Martins, M., Machado, J., & Baptista, L. (2021). *Predict Students' Dropout and Academic Success* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5MC89
- Martins, M. V., Tolledo, D., Machado, J., Baptista, L. M. T., & Realinho, V. (2021). “Early prediction of student's performance in higher education: a case study”. *Trends and Applications in Information Systems and Technologies*. https://doi.org/10.1007/978-3-030-72657-7_16
- Licencia del dataset: [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/legalcode).
