# Segmentación ecográfica del recto femoral con Deep Learning (U-Net, Attention U-Net y U-Net++)



Este repositorio contiene el trabajo realizado en el TFM orientado a la **segmentación automática del músculo recto femoral** en imágenes de ecografía de adultos mayores mediante **redes neuronales profundas**. El objetivo es generar **máscaras de segmentación** y derivar **parámetros morfométricos** (p. ej., área transversal y grosor/profundidad) para apoyar el análisis de sarcopenia.



Se han evaluado tres arquitecturas de segmentación biomédica:

- **U-Net** (baseline)

- **Attention U-Net**

- **U-Net++**



El entrenamiento se realizó sobre un subconjunto anotado de **450 imágenes con máscara manual**, y posteriormente los modelos se aplicaron a la cohorte completa de **4.500 imágenes** para inferencia y extracción morfométrica.



> **Nota importante (datos y privacidad):** las imágenes ecográficas provienen de un entorno clínico y **no se incluyen** en este repositorio. Se proporcionan scripts/notebooks y estructura de carpetas para reproducir el pipeline con datos locales.



---



## Contenido del repositorio



- `notebooks/`

&nbsp; - Notebooks de preprocesado, entrenamiento, inferencia y análisis comparativo.

- `html/`

&nbsp; - Notebooks guardados en formato HTML con la ejecución y los resultados obtenidos.

- `results/`

&nbsp;- Tablas y métricas exportadas (CSV/JSON), sin información identificable.

- `requirements.txt`

&nbsp; - Dependencias para reproducibilidad.



---



## Pipeline (visión general)



1. **Construcción y preprocesado del dataset**

&nbsp;  - Normalización / estandarización de tamaño

&nbsp;  - Limpieza de anotaciones superpuestas (si aplica)

&nbsp;  - Preparación de pares imagen–máscara para entrenamiento supervisado\\



2. **Entrenamiento**

&nbsp;  - Modelos: U-Net, Attention U-Net, U-Net++

&nbsp;  - Pérdida: Dice Loss

&nbsp;  - Optimizador: Adam (lr inicial 1e-4)

&nbsp;  - Batch size: 8

&nbsp;  - Early stopping sobre validación



3. **Evaluación**

&nbsp;  - Métricas principales: **Dice** e **IoU**

&nbsp;  - Evaluación global y por centro

&nbsp;  - Comparación estadística (Wilcoxon pareado) entre arquitecturas



4. **Inferencia sobre la cohorte completa (4.500 imágenes)**

&nbsp;  - Generación automática de máscaras

&nbsp;  - Cálculo de parámetros morfométricos:

&nbsp;    - Área transversal (cm²)

&nbsp;    - Grosor / profundidad (cm)



> **Disclaimer de calidad (inferencia masiva):** las medidas en la cohorte de 4.500 imágenes se obtuvieron de forma **automática** sin revisión manual caso a caso. Se han observado casos puntuales con valores atípicos (p. ej., mínimos 0), atribuibles a fallos de segmentación o baja calidad de imagen. Se recomienda limpieza/QA adicional para usos clínicos.



---



## Estructura esperada de datos (local)



Crea una carpeta `data/` (ignoradas por git) con una estructura similar:

data/<br>
	processed/<br>
		<CENTRO_1>.jpeg<br>
		<CENTRO_2>.jpeg<br>
	masks_manual/<br>
		<CENTRO_1>.png<br>
		<CENTRO_2>.png<br>
	pred_masks/<br>
		UNet/<br>
			<CENTRO_1>.png<br>
		AttentionUNet/<br>
			<CENTRO_1>.png<br>
		UNetPlusPlus/<br>
			<CENTRO_1>.png<br>

> Ajusta rutas según tu entorno. En los notebooks hay celdas para configurar paths base.

---



## Instalación

Para reproducir el entorno de ejecución se recomienda utilizar un entorno virtual
con Python 3.9 o superior.

### Opción A: instalación con pip

``bash
python -m venv .venv

#### Windows: .venvScriptsactivate
#### Linux/Mac: source .venv/bin/activate
pip install -r requirements.txt


### Opción B: `conda`

conda env create -f environment.yml
conda activate rf-us-seg



## Ejecución (orden recomendado)

1. Preprocesado del dataset
2. Entrenamiento de los modelos (U-Net, Attention U-Net, U-Net++)
3. Inferencia sobre la cohorte completa
4. Análisis comparativo de resultados



## Reproducibilidad y consideraciones metodológicas

La partición entrenamiento/validación/test se realizó a nivel de imagen debido a la
anonimización de los datos. En futuros trabajos se recomienda partición agrupada por
sujeto y validación cruzada estratificada por centro.













