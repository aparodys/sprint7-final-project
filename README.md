# sprint7-final-project

# Análisis de uso de clientes - ConnectaTel

Proyecto final del Sprint 7 del bootcamp de Análisis de Datos de TripleTen.

## Sobre el proyecto

En este proyecto trabajé como analista de datos de ConnectaTel, una empresa de telecomunicaciones que opera en México y Colombia. La idea era entender cómo los clientes usan realmente sus servicios de llamadas y mensajes, encontrar comportamientos raros en los datos y ver si hay segmentos de clientes con necesidades distintas, para proponer mejoras a los planes que ofrece la empresa.

Las preguntas que guiaron el análisis fueron:

- ¿Qué segmentos de clientes usan más o menos llamadas y mensajes?
- ¿Hay usuarios con valores atípicos que puedan indicar errores de registro o fraude?
- ¿Cómo cambia el uso según la edad y el plan contratado?
- ¿Qué patrones pueden servir para diseñar mejores planes?

## Datos

Usé tres archivos CSV:

- **plans.csv** (2 filas): los dos planes disponibles, Basico y Premium, con su precio, minutos, mensajes y GB incluidos, y el costo por excedente.
- **users_latam.csv** (4,000 filas): datos de los clientes, como edad, ciudad, fecha de registro, plan y fecha de baja si cancelaron.
- **usage.csv** (40,000 filas): cada llamada o mensaje registrado entre enero y junio de 2024, con la duración de las llamadas y la longitud de los mensajes.

## Qué hice

1. Cargué los tres datasets y revisé su estructura y tipos de datos.
2. Busqué problemas de calidad: valores nulos, valores sentinel (-999 en la edad, "?" en la ciudad, 120 en la duración y 1490 en la longitud) y fechas imposibles (40 clientes registrados en 2026).
3. Limpié los datos: reemplacé los sentinels, convertí las fechas a formato datetime y marqué como nulas las fechas fuera de rango. También comprobé que los nulos en `duration` y `length` dependen del tipo de evento (MAR), así que los dejé como estaban.
4. Armé una tabla por usuario con el total de mensajes, llamadas y minutos, y la uní con la información de los clientes.
5. Hice histogramas por plan y boxplots, y calculé los límites con el método IQR para identificar outliers.
6. Segmenté a los clientes por nivel de uso (bajo, medio y alto) y por edad (joven, adulto y adulto mayor).
7. Escribí las conclusiones y recomendaciones para el negocio.

## Resultados principales

- Los clientes usan mucho menos de lo que incluyen sus planes. Un cliente típico usó unos 20 minutos y 5 mensajes en todo el periodo, cuando el plan Basico incluye 100 minutos y 100 mensajes al mes.
- No encontré diferencias de uso entre planes: los clientes Premium hacen prácticamente las mismas llamadas y envían los mismos mensajes que los de plan Basico.
- El 73.6 % de los clientes tiene un uso medio, el 19.5 % un uso bajo y solo el 7 % un uso alto. Este último grupo es el que más cancela (14 % de churn).
- Hay 266 clientes Premium con uso bajo, que podrían bajar de plan o cancelar.
- Unos 30 usuarios tienen minutos inflados por un error de registro (llamadas guardadas con 120 minutos).

## Cómo correr el notebook

**En Google Colab:**

1. Entra a Colab y abre el notebook desde la pestaña GitHub (File > Open notebook > GitHub) pegando la URL de este repositorio.
2. Sube los tres CSV a la sesión dentro de una carpeta `datasets`.
3. Cambia las rutas de carga (ver abajo) y ejecuta todo con Runtime > Run all.

**En Jupyter local:**

```bash
git clone https://github.com/aparodys/sprint7-final-project.git
cd sprint7-final-projec
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook
```

Después abre el notebook y ejecuta Kernel > Restart & Run All.

## Notas para reproducirlo

El notebook carga los datos desde `/datasets/`, que es la ruta de la plataforma de TripleTen. Si lo corres en otro lado, hay que cambiar las rutas a donde tengas los archivos, por ejemplo:

```python
plans = pd.read_csv('datasets/plans.csv')
users = pd.read_csv('datasets/users_latam.csv')
usage = pd.read_csv('datasets/usage.csv')
```

Las celdas se tienen que ejecutar en orden, porque cada paso usa las columnas y la limpieza de los pasos anteriores.

Librerías usadas: Python 3.9, pandas, numpy, matplotlib y seaborn.

## Autor

Angelica Parodys
