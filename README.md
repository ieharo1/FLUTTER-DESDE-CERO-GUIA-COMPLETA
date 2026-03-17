# ⚡ APACHE SPARK DESDE CERO - GUÍA COMPLETA

**Apache Spark desde Cero** es un sitio educativo completo diseñado para enseñar Spark desde los fundamentos hasta conceptos avanzados, con explicaciones claras, ejemplos prácticos y código listo para usar.

> *"Apache Spark is a unified analytics engine for large-scale data processing."*

---

## 🎯 ¿Qué es este Proyecto?

Este proyecto proporciona un recurso educativo gratuito para aprender Apache Spark, incluyendo:

- **Documentación completa** de cada tema
- **Ejemplos de código** listos para ejecutar
- **Ejercicios prácticos** para reforzar el aprendizaje
- **Sitio web educativo** con navegación intuitiva

---

## 📚 Contenido del Curso

### Módulo 1: Fundamentos

1. **Introducción**
   - ¿Qué es Apache Spark?
   - Spark vs Hadoop MapReduce
   - Ecosistema Spark

2. **Instalación**
   - Spark standalone
   - Spark en cluster
   - Databricks Community
   - PySpark setup

3. **Conceptos básicos**
   - RDDs (Resilient Distributed Datasets)
   - Transformaciones y acciones
   - Lazy evaluation
   - SparkContext y SparkSession

### Módulo 2: Intermedio

4. **Ejemplos prácticos**
   - DataFrames y Datasets
   - Spark SQL
   - Lectura/escritura de datos
   - UDFs (User Defined Functions)

5. **Buenas prácticas**
   - Optimización de queries
   - Partitioning y bucketing
   - Caching y persistencia
   - Manejo de memoria

### Módulo 3: Avanzado

6. **Casos reales**
   - Spark Streaming
   - Structured Streaming
   - MLlib (Machine Learning)
   - GraphX

7. **Proyecto final**
   - Pipeline de datos completo
   - Deploy en cluster
   - Monitoreo con Spark UI

---

## 🗂️ Estructura del Proyecto

```
MySQL-/
├── index.html          # Página principal
├── css/
│   └── styles.css      # Estilos del sitio
├── js/
│   └── main.js         # JavaScript del sitio
└── README.md
```

---

## 🚀 Cómo Usar este Proyecto

### Opción 1: Navegar el Sitio Web

1. Abre `index.html` en tu navegador
2. Navega por las secciones del curso
3. Haz clic en los temas para ver la documentación detallada

### Opción 2: Ejecutar los Ejemplos

1. Instala Spark o usa Databricks
2. Crea script .py o .scala
3. Ejecuta con `spark-submit`

### Requisitos

- Java 8 o superior
- Python 3.7+ (para PySpark)
- 4GB RAM mínimo

---

## 📝 Ejemplos Rápidos

### Inicialización PySpark

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("Mi Aplicación") \
    .master("local[*]") \
    .getOrCreate()

df = spark.read.csv("datos.csv", header=True, inferSchema=True)
df.show()
```

### Transformaciones y Acciones

```python
# Transformaciones (lazy)
filtered = df.filter(df.edad > 18)
selected = df.select("nombre", "email")
grouped = df.groupBy("ciudad").count()

# Acciones (eager)
count = df.count()
primeros = df.head(5)
total = df.agg({"salario": "sum"}).collect()
```

### Spark SQL

```python
# Registrar como tabla temporal
df.createOrReplaceTempView("usuarios")

# Ejecutar SQL
resultado = spark.sql("""
    SELECT ciudad, AVG(salario) as promedio
    FROM usuarios
    WHERE edad > 18
    GROUP BY ciudad
""")
```

### MLlib Ejemplo

```python
from pyspark.ml.classification import LogisticRegression
from pyspark.ml.feature import VectorAssembler

# Preparar features
assembler = VectorAssembler(
    inputCols=["edad", "ingresos"],
    outputCol="features"
)

data = assembler.transform(df)

# Entrenar modelo
lr = LogisticRegression(featuresCol="features", labelCol="label")
modelo = lr.fit(data)

# Predecir
predicciones = modelo.transform(data)
```

### Structured Streaming

```python
# Leer stream
lines = spark.readStream \
    .format("socket") \
    .option("host", "localhost") \
    .option("port", 9999) \
    .load()

# Procesar
palabras = lines.selectExpr("explode(split(value, ' ')) as palabra")

# Escribir stream
query = palabras.writeStream \
    .outputMode("complete") \
    .format("console") \
    .start()

query.awaitTermination()
```

---

## 🎓 Metodología de Aprendizaje

### 1. Leer la Teoría
Cada tema comienza con una explicación clara del concepto.

### 2. Ver Ejemplos
Los ejemplos de código muestran la aplicación práctica.

### 3. Practicar
Los ejercicios te permiten aplicar lo aprendido.

### 4. Experimentar
Modifica los ejemplos para entender cómo funcionan.

---

## 🔧 Comandos Esenciales

### Terminal/Consola

```bash
# Iniciar Spark shell
spark-shell

# Iniciar PySpark
pyspark

# Ejecutar aplicación
spark-submit --master local[4] app.py

# Ver logs
$SPARK_HOME/logs/

# Spark submit con opciones
spark-submit \
  --class com.example.Main \
  --master yarn \
  --deploy-mode cluster \
  app.jar
```

---

## 📖 Recursos Adicionales

### Documentación Oficial

- [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
- [PySpark API](https://spark.apache.org/docs/latest/api/python/)
- [Databricks Learning](https://academy.databricks.com/)

### Herramientas Recomendadas

- **Databricks Community** - IDE cloud gratuito
- **Apache Zeppelin** - Notebook web
- **Jupyter + Spark** - Notebook local

### Comunidades

- [Spark Community](https://spark.apache.org/community.html)
- [Stack Overflow - Apache Spark](https://stackoverflow.com/questions/tagged/apache-spark)
- [Reddit r/spark](https://www.reddit.com/r/spark/)

---

## 💡 Consejos para Principiantes

1. **Entiende lazy evaluation**: Las transformaciones no se ejecutan hasta una acción.
2. **Usa DataFrames**: Son más optimizados que RDDs.
3. **Cachea inteligentemente**: Solo datos reutilizados.
4. **Partitiona correctamente**: Evita data skew.
5. **Monitoriza con Spark UI**: Es tu mejor amigo.

---

## ⚠️ Mejores Prácticas

### Rendimiento

- Usa broadcast joins para tablas pequeñas
- Evita collect() en datos grandes
- Reparticiona solo cuando sea necesario

### Memoria

- Ajusta executor memory según necesidad
- Usa serialize eficiente (Kryo)
- Limpia cache cuando no se use

### Producción

- Usa cluster mode en producción
- Implementa checkpointing en streaming
- Configura log levels apropiadamente

---

## 🧪 Ejercicios Prácticos

### Nivel Básico

1. Carga y explora un dataset CSV
2. Filtra y transforma datos
3. Agrega datos por categoría

### Nivel Intermedio

1. Pipeline ETL completo
2. Modelo de ML con MLlib
3. Join de múltiples fuentes

### Nivel Avanzado

1. Streaming en tiempo real
2. Procesamiento de logs masivos
3. Graph analysis con GraphX

---

## 👨‍💻 Desarrollado por Isaac Esteban Haro Torres

**Ingeniero en Sistemas · Full Stack · Automatización · Data**

- 📧 Email: zackharo1@gmail.com
- 📱 WhatsApp: 098805517
- 💻 GitHub: https://github.com/ieharo1
- 🌐 Portafolio: https://ieharo1.github.io/portafolio-isaac.haro/

---

© 2026 Isaac Esteban Haro Torres - Todos los derechos reservados.
