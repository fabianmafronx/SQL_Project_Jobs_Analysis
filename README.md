# Introduccion
📊 ¡Sumérgete en el mercado laboral de datos!
Enfocado en roles de analista de datos, este proyecto explora 💰 los empleos mejor pagados, 🔥 las habilidades más demandadas y 📈 dónde la alta demanda se encuentra con los altos salarios en el campo del análisis de datos.

🔍 ¿Consultas SQL? Revísalas aquí: [carpeta proyecto_sql](/proyecto_sql/).

# 🧠 Contexto
Impulsado por el objetivo de navegar de manera más efectiva el mercado laboral de analistas de datos, este proyecto nació del deseo de identificar las habilidades mejor pagadas y más demandadas, facilitando el trabajo de otros para encontrar oportunidades laborales óptimas.

Los datos provienen del curso de SQL de https://www.youtube.com/watch?v=7mz73uXD9DA&list=PL_CkpxkuPiT-RJ7zBfHVWwgltEWIVwrwb&index=4 y están llenos de información sobre:

### Las preguntas que quise responder a través de mis consultas en SQL fueron:

¿Cuáles son los empleos mejor pagados para los analistas de datos?

¿Qué habilidades se requieren para estos empleos mejor pagados?

¿Qué habilidades son las más demandadas para los analistas de datos?

¿Qué habilidades están asociadas con salarios más altos?

¿Cuáles son las habilidades más óptimas para aprender?

# Herramientas que utilicé
Para mi análisis profundo del mercado laboral de analistas de datos, aproveché el poder de varias herramientas clave:

SQL: La base de mi análisis, que me permitió consultar la base de datos y descubrir insights críticos.

PostgreSQL: El sistema de gestión de bases de datos elegido, ideal para manejar los datos de ofertas de empleo.

Visual Studio Code: Mi herramienta principal para la gestión de bases de datos y la ejecución de consultas SQL.

Git y GitHub: Esenciales para el control de versiones y para compartir mis scripts SQL y análisis, asegurando la colaboración y el seguimiento del proyecto.

# El analisis
Cada consulta de este proyecto tuvo como objetivo investigar aspectos específicos del mercado laboral de analistas de datos. Así es como abordé cada pregunta:

### 1. Empleos mejor pagados para Analistas de Datos

Para identificar los roles con los salarios más altos, filtré las posiciones de analista de datos por salario promedio anual y ubicación, enfocándome en trabajos remotos. Esta consulta resalta las oportunidades mejor remuneradas dentro del campo.

```sql
SELECT	
	job_id,
	job_title,
	job_location,
	job_schedule_type,
	salary_year_avg,
	job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND 
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

Aquí está el desglose de los principales empleos de analista de datos en 2023:

Amplio rango salarial: Los 10 roles mejor pagados para analistas de datos oscilan entre $184,000 y $650,000, lo que indica un alto potencial salarial en el campo.

Empleadores diversos: Empresas como SmartAsset, Meta y AT&T se encuentran entre las que ofrecen salarios elevados, lo que demuestra un interés amplio en distintas industrias.

Variedad de títulos de trabajo: Existe una gran diversidad de cargos, desde Analista de Datos hasta Director de Analítica, lo que refleja la variedad de roles y especializaciones dentro del análisis de datos.

![Top Paying Skills](recursos/1_top_paying_roles.png)
*Bar graph visualizing the count of skills for the top 10 paying jobs for data analysts; ChatGPT generated this graph from my SQL query results*


# Que aprendi

# Conclusiones
