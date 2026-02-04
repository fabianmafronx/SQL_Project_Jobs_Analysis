# Introduccion
📊 ¡Sumérgete en el mercado laboral de datos!
Enfocado en roles de analista de datos, este proyecto explora 💰 los empleos mejor pagados, 🔥 las habilidades más demandadas y 📈 dónde la alta demanda se encuentra con los altos salarios en el campo del análisis de datos.

🔍 ¿Consultas SQL? Revísalas aquí: [carpeta proyecto_sql](/proyecto_sql/).

# 🧠 Contexto
Impulsado por el objetivo de navegar de manera más efectiva el mercado laboral de analistas de datos, este proyecto nació del deseo de identificar las habilidades mejor pagadas y más demandadas, facilitando el trabajo de otros para encontrar oportunidades laborales óptimas.

Los datos provienen del curso de SQL de https://www.youtube.com/watch?v=7mz73uXD9DA&list=PL_CkpxkuPiT-RJ7zBfHVWwgltEWIVwrwb&index=4 y están llenos de información sobre:

### Las preguntas que quise responder a través de mis consultas en SQL fueron:

- 1.¿Cuáles son los empleos mejor pagados para los analistas de datos?

- 2.¿Qué habilidades se requieren para estos empleos mejor pagados?

- 3.¿Qué habilidades son las más demandadas para los analistas de datos?

- 4.¿Qué habilidades están asociadas con salarios más altos?

- 5.¿Cuáles son las habilidades más óptimas para aprender?

# Herramientas que utilicé
Para mi análisis profundo del mercado laboral de analistas de datos, aproveché el poder de varias herramientas clave:

- **SQL:** La base de mi análisis, que me permitió consultar la base de datos y descubrir insights críticos.

- **PostgreSQL:** El sistema de gestión de bases de datos elegido, ideal para manejar los datos de ofertas de empleo.

- **Visual Studio Code:** Mi herramienta principal para la gestión de bases de datos y la ejecución de consultas SQL.

- **Git y GitHub:** Esenciales para el control de versiones y para compartir mis scripts SQL y análisis, asegurando la colaboración y el seguimiento del proyecto.

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

- **Amplio rango salarial:** Los 10 roles mejor pagados para analistas de datos oscilan entre $184,000 y $650,000, lo que indica un alto potencial salarial en el campo.

- **Empleadores diversos:** Empresas como SmartAsset, Meta y AT&T se encuentran entre las que ofrecen salarios elevados, lo que demuestra un interés amplio en distintas industrias.

- **Variedad de títulos de trabajo:** Existe una gran diversidad de cargos, desde Analista de Datos hasta Director de Analítica, lo que refleja la variedad de roles y especializaciones dentro del análisis de datos.

![Top Paying Skills](recursos/1_top_paying_roles.png)
*Gráfico de barras que visualiza la cantidad de habilidades para los 10 empleos mejor pagados de analista de datos; ChatGPT generó este gráfico a partir de los resultados de mis consultas SQL*

### 2. Habilidades para los empleos mejor pagados

Para entender qué habilidades se requieren para los empleos mejor pagados, uní las ofertas de trabajo con los datos de habilidades, lo que proporciona insights sobre lo que los empleadores valoran en los roles con alta compensación.

```sql
WITH top_paying_jobs AS (
    SELECT	
        job_id,
        job_title,
        salary_year_avg,
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
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```
Aquí está el desglose de las habilidades más demandadas para los 10 empleos mejor pagados de analista de datos en 2023:

- **SQL** lidera con una fuerte presencia, con un conteo de 8.

- **Python** le sigue de cerca, con un conteo de 7.

- **Tableau** también es altamente solicitado, con un conteo de 6.

Otras habilidades como **R**, **Snowflake**, **Pandas** y **Excel** muestran distintos niveles de demanda.

![Top Paying Skills](recursos/2_top_paying_roles_skills.png)
*Gráfico de barras que visualiza la cantidad de habilidades para los 10 empleos mejor pagados de analista de datos; ChatGPT generó este gráfico a partir de los resultados de mis consultas SQL*

### 3. Habilidades más demandadas para Analistas de Datos

Esta consulta ayudó a identificar las habilidades solicitadas con mayor frecuencia en las ofertas de empleo, orientando el enfoque hacia las áreas con mayor demanda.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
Aquí está el desglose de las habilidades más demandadas para analistas de datos en 2023:

- **SQL** y **Excel** siguen siendo fundamentales, lo que enfatiza la necesidad de contar con bases sólidas en el procesamiento de datos y la manipulación de hojas de cálculo.

- **Programación** y **herramientas de visualización** como **Python**, **Tableau** y **Power BI** son esenciales, lo que señala la creciente importancia de las habilidades técnicas en la narrativa de datos y el apoyo a la toma de decisiones.

| Skills   | Demand Count |
|----------|--------------|
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |

*Tabla de la demanda de las 5 principales habilidades en las ofertas de empleo para analistas de datos*

### 4. Habilidades basadas en el salario
Explorar los salarios promedio asociados a diferentes habilidades permitió identificar cuáles son las habilidades mejor pagadas.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
Aquí está el desglose de los resultados sobre las habilidades mejor pagadas para Analistas de Datos:

- **Alta demanda de habilidades en Big Data y Machine Learning:** Los salarios más altos son obtenidos por analistas con conocimientos en tecnologías de big data (PySpark, Couchbase), herramientas de machine learning (DataRobot, Jupyter) y librerías de Python (Pandas, NumPy), lo que refleja la alta valoración de la industria por las capacidades de procesamiento de datos y modelado predictivo.

- **Dominio en desarrollo de software y despliegue:** El conocimiento en herramientas de desarrollo y despliegue (GitLab, Kubernetes, Airflow) indica una convergencia rentable entre el análisis de datos y la ingeniería, con una prima sobre las habilidades que facilitan la automatización y la gestión eficiente de pipelines de datos.

- **Experiencia en computación en la nube:** La familiaridad con herramientas de computación en la nube y de ingeniería de datos (Elasticsearch, Databricks, GCP) subraya la creciente importancia de los entornos analíticos basados en la nube, lo que sugiere que la competencia en la nube incrementa significativamente el potencial de ingresos en el análisis de datos.

| Skills        | Average Salary ($) |
|---------------|-------------------:|
| pyspark       |            208,172 |
| bitbucket     |            189,155 |
| couchbase     |            160,515 |
| watson        |            160,515 |
| datarobot     |            155,486 |
| gitlab        |            154,500 |
| swift         |            153,750 |
| jupyter       |            152,777 |
| pandas        |            151,821 |
| elasticsearch |            145,000 |

*Tabla del salario promedio de las 10 habilidades mejor pagadas para analistas de datos*

### 5. Habilidades más óptimas para aprender

Al combinar insights de la demanda y los datos salariales, esta consulta tuvo como objetivo identificar las habilidades que tienen alta demanda y altos salarios, ofreciendo un enfoque estratégico para el desarrollo de habilidades.

```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

| Skill ID | Skills     | Demand Count | Average Salary ($) |
|----------|------------|--------------|-------------------:|
| 8        | go         | 27           |            115,320 |
| 234      | confluence | 11           |            114,210 |
| 97       | hadoop     | 22           |            113,193 |
| 80       | snowflake  | 37           |            112,948 |
| 74       | azure      | 34           |            111,225 |
| 77       | bigquery   | 13           |            109,654 |
| 76       | aws        | 32           |            108,317 |
| 4        | java       | 17           |            106,906 |
| 194      | ssis       | 12           |            106,683 |
| 233      | jira       | 20           |            104,918 |

*Tabla de las habilidades más óptimas para analistas de datos ordenadas por salario*

Aquí está el desglose de las habilidades más óptimas para Analistas de Datos en 2023:

- **Lenguajes de programación con alta demanda:** Python y R destacan por su alta demanda, con conteos de 236 y 148 respectivamente. A pesar de esta alta demanda, sus salarios promedio rondan los $101,397 para Python y $100,499 para R, lo que indica que la competencia en estos lenguajes es altamente valorada, pero también ampliamente disponible.

- **Herramientas y tecnologías en la nube:** Habilidades en tecnologías especializadas como Snowflake, Azure, AWS y BigQuery muestran una demanda significativa con salarios promedio relativamente altos, lo que apunta a la creciente importancia de las plataformas en la nube y las tecnologías de big data en el análisis de datos.

- **Herramientas de inteligencia de negocios y visualización:** Tableau y Looker, con conteos de demanda de 230 y 49 respectivamente, y salarios promedio de alrededor de $99,288 y $103,795, destacan el papel crítico de la visualización de datos y la inteligencia de negocios para obtener insights accionables a partir de los datos.

- **Tecnologías de bases de datos:** La demanda de habilidades en bases de datos tradicionales y NoSQL (Oracle, SQL Server, NoSQL) con salarios promedio que oscilan entre $97,786 y $104,534, refleja la necesidad constante de experiencia en almacenamiento, recuperación y gestión de datos.

# Que aprendi

A lo largo de esta aventura, potencié al máximo mi kit de herramientas en SQL con auténtico poder:

- **🧩 Creación de consultas complejas:** Dominé el arte del SQL avanzado, uniendo tablas como un profesional y utilizando cláusulas WITH para maniobras de tablas temporales a nivel ninja.

- **📊 Agregación de datos:** Me familiaricé con GROUP BY y convertí funciones agregadas como COUNT() y AVG() en mis aliados para resumir datos.

- **💡 Magia analítica:** Elevé mis habilidades de resolución de problemas del mundo real, transformando preguntas en consultas SQL accionables y llenas de insight.

# Conclusiones

### Insights

Del análisis surgieron varios insights generales:

1. **Empleos mejor pagados para Analistas de Datos:** Los trabajos mejor remunerados para analistas de datos que permiten trabajo remoto ofrecen un amplio rango salarial, ¡alcanzando hasta $650,000!

2. **Habilidades para empleos mejor pagados:** Los empleos de analista de datos con altos salarios requieren un dominio avanzado de SQL, lo que sugiere que es una habilidad crítica para alcanzar una compensación elevada.

3. **Habilidades más demandadas:** SQL también es la habilidad más solicitada en el mercado laboral de analistas de datos, lo que la convierte en esencial para quienes buscan empleo.

4. **Habilidades con salarios más altos:** Habilidades especializadas, como SVN y Solidity, están asociadas con los salarios promedio más altos, lo que indica una prima por conocimientos de nicho.

5. **Habilidades óptimas para maximizar el valor en el mercado laboral:** SQL lidera tanto en demanda como en salarios promedio elevados, posicionándola como una de las habilidades más óptimas que un analista de datos puede aprender para maximizar su valor en el mercado. 

### Reflexiones finales

Este proyecto fortaleció mis habilidades en SQL y proporcionó insights valiosos sobre el mercado laboral de analistas de datos. Los hallazgos del análisis sirven como una guía para priorizar el desarrollo de habilidades y los esfuerzos de búsqueda de empleo.Los aspirantes a analistas de datos pueden posicionarse mejor en un mercado laboral competitivo al enfocarse en habilidades con alta demanda y altos salarios. Esta exploración resalta la importancia del aprendizaje continuo y la adaptación a las tendencias emergentes en el campo del análisis de datos.