# 🦟 Dengue en Loreto (2000–2024): ¿cuándo, dónde y a quiénes afecta más?

![Estado](https://img.shields.io/badge/Estado-En%20progreso-yellow?style=flat-square)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)

Análisis de **107,074 casos de dengue** notificados entre 2000 y 2024 con lugar probable de infección en Loreto, a partir de los datos abiertos de vigilancia epidemiológica del MINSA.

## 🎯 Pregunta de análisis

¿Cuándo, dónde y a quiénes afecta más el dengue en Loreto, y qué cambió en las epidemias de 2023 y 2024?

Loreto es el **tercer departamento del Perú con más casos de dengue** registrados entre 2000 y 2024, detrás de Piura y Lima, y concentra el **10.4%** del total nacional.

## 📊 Resultados principales

> 🚧 En construcción. Esta sección se completará al terminar el análisis.

**Primeros hallazgos** (notebooks 01 y 02):

1. **2023 y 2024 fueron años excepcionales a nivel nacional:** juntos suman más de 528,000 casos, más de la mitad de todo el registro de 25 años. 2024 (271,531 casos) superó incluso a 2023 (256,641).
2. **El dengue en Loreto afecta sobre todo a población joven:** la edad mediana de los casos es de **21 años**, y el **42.7%** son menores de 18.
3. **Los casos en Loreto son, en proporción, más graves que el promedio nacional:** el **15.5%** presenta signos de alarma o es grave, frente al **11.1%** en todo el país.
4. **El 52.4% de los casos corresponde a mujeres.**

<!-- Aquí irán la curva epidémica, el heatmap de semanas por año y los hallazgos por distrito -->

## 💡 Implicaciones

> 🚧 En construcción.

## 🗂️ Datos

| | |
|---|---|
| **Fuente** | [Vigilancia epidemiológica de dengue – Plataforma Nacional de Datos Abiertos](https://www.datosabiertos.gob.pe/dataset/vigilancia-epidemiol%C3%B3gica-de-dengue) |
| **Publicado por** | Centro Nacional de Epidemiología, Prevención y Control de Enfermedades (CDC Perú – MINSA) |
| **Archivo** | `datos_abiertos_vigilancia_dengue_2000_2024.csv` (103 MB) |
| **Cobertura** | Perú, 2000–2024 · 1,029,421 casos notificados |
| **Unidad de análisis** | Un caso de dengue por fila |
| **Filtro aplicado** | Lugar probable de infección: departamento de Loreto (107,074 casos) |

**Variables utilizadas:** provincia, distrito y ubigeo (lugar probable de infección), año y semana epidemiológica de inicio de síntomas, forma clínica, dirección de salud que notifica, edad y sexo.

> ⚠️ El archivo original no se incluye en el repositorio porque supera el límite de 100 MB de GitHub. Para reproducir el análisis, descárgalo desde el enlace de la fuente y guárdalo en `data/raw/`.

## 🧹 Metodología

**Exploración y calidad de datos** (`01_exploracion.ipynb`)

1. Identificación del separador (`;`) y detección de la codificación del archivo.
2. Carga indicando como texto los códigos (`ubigeo`, `diresa`) y la edad, para no perder ceros a la izquierda ni mezclar unidades.
3. Comparación de las columnas reales con el diccionario de datos: falta `tipo_dx` y sobran `localidad` y `localcod`.
4. Diagnóstico y corrección de nombres con caracteres dañados (ñ y tildes), usando el `ubigeo` para recuperar la versión correcta de cada nombre dentro del propio dataset.
5. Validación del total de 2023 contra la cifra reportada para ese año: el dataset contiene alrededor del **94%** de los casos.
6. Filtro de Loreto por lugar probable de infección (`departamento`), no por la dirección de salud que notifica (`diresa`).

**Limpieza y preparación** (`02_limpieza.ipynb`)

7. Conversión de todas las edades a años: la edad viene registrada en años, meses o días según `tipo_edad`.
8. Validación de edades: rango de 0 a 104 años, sin valores imposibles.
9. Grupos de edad según las etapas de vida del MINSA: niño, adolescente, joven, adulto y adulto mayor.
10. Gravedad como variable categórica ordenada: sin signos de alarma, con signos de alarma y grave.
11. Validación de sexo y semanas epidemiológicas, y comprobación de que la limpieza no eliminó ningún caso.

**Próximas etapas**

12. Carga en una base de datos SQLite y análisis con consultas SQL (`03_sql.ipynb`).
13. Visualización y conclusiones (`04_visualizacion.ipynb`).
14. Dashboard interactivo en Power BI.

## ⚠️ Limitaciones

- **No se distinguen casos confirmados, probables y sospechosos:** el archivo no incluye la columna `tipo_dx` descrita en el diccionario de datos.
- **No hay información sobre fallecimientos**, por lo que no se puede analizar la letalidad.
- **El lugar registrado es el de probable infección**, no el de residencia del paciente.
- **Solo se conoce la semana de inicio de síntomas**, no la fecha exacta.
- **Los conteos de casos no equivalen a riesgo:** para comparar distritos o grupos de edad hace falta la población de cada uno (incidencia por 100,000 habitantes), que se incorporará con datos del INEI.
- Las filas idénticas no se eliminan: sin un identificador de paciente, no es posible distinguir un registro duplicado de dos casos reales con las mismas características.

## 🔒 Consideraciones éticas

Aunque los datos son públicos y anonimizados, corresponden a personas reales. Por eso todos los resultados se presentan **agregados** (por distrito, semana, año o grupo de edad), nunca como casos individuales, y no se utiliza la columna `localidad`, que combinada con edad y sexo podría acercarse a identificar personas en comunidades pequeñas.

## 📁 Estructura del proyecto

```
dengue-loreto/
├── data/
│   ├── raw/                            # Datos originales del MINSA (no se suben a GitHub)
│   └── processed/
│       ├── dengue_loreto.csv           # Casos de Loreto, columnas seleccionadas
│       └── dengue_loreto_limpio.csv    # Edad en años, grupos de edad y gravedad
├── images/                             # Gráficos del análisis
├── notebooks/
│   ├── 01_exploracion.ipynb            # Carga, calidad de datos y filtro de Loreto
│   └── 02_limpieza.ipynb               # Edades, grupos de edad y gravedad
├── sql/                                # Consultas SQL (próximamente)
├── requirements.txt
└── README.md
```

## ▶️ Cómo reproducirlo

```bash
git clone https://github.com/David-Sac/dengue-loreto.git
cd dengue-loreto
python -m venv .venv
.venv\Scripts\activate          # En Windows
pip install -r requirements.txt
```

Luego descarga el archivo de la fuente, guárdalo en `data/raw/` y ejecuta los notebooks en orden.

## 👤 Autor

**David Saccsara** · Data Analyst en formación · Maestría en Ciencia de Datos e IA (UTP, en curso)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/davidsacc/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/David-Sac)