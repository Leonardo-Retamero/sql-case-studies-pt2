### SQL Cases - Parte 2

Este repositório reúne exercícios resolvidos (cases) do curso de SQL para Análise de Dados oferecido pela Fundação Getulio Vargas (FGV). O objetivo é aplicar na prática os conceitos aprendidos ao longo do curso.

Cada case apresenta:

Um enunciado prático

O código SQL utilizado na resolução

Uma breve análise do resultado

---

### 📌 Case 1 — Evolução da Expectativa de Vida por Região (2019–2021) 

Analise a expectativa de vida média, entre os anos de 2019 a 2021, agrupando os países por suas respectivas oito regiões geográficas, conforme uma classificação internacional. A consulta deve apresentar os valores médios por região e ano, permitindo observar possíveis impactos recentes sobre a longevidade da população. 

💻 Código SQL: 

```sql
SELECT le.ref_year,
	   c.eight_regions,
	   ROUND(AVG(le.tot_years),2) AS life_expectancy
FROM life_expectancy le 
JOIN country c ON le.country = c.country
WHERE le.ref_year BETWEEN 2019 AND 2021 
GROUP BY c.eight_regions, le.ref_year
ORDER BY c.eight_regions, le.ref_year;
```

📊 Análise:

Essa consulta calcula a expectativa média de vida entre 2019 e 2021 para cada uma das oito grandes regiões mundiais, conforme categorização de origem externa. Os dados são agrupados por ano e região, o que permite visualizar tendências recentes, especialmente os impactos gerados pela pandemia de COVID-19.

📌 Principais observações:

• A maioria das regiões apresentou um declínio constante na expectativa de vida ao longo dos três anos.

• A Europa Ocidental foi a única exceção, com uma redução de 2019 para 2020, seguida por um pequeno aumento em 2021, sugerindo uma possível recuperação inicial após os impactos mais severos da pandemia.

• Essa análise destaca a importância de segmentar dados por regiões para compreender variações nos efeitos de eventos globais sobre indicadores de saúde.

🎯 Resultado: 

<img width="423" height="483" alt="image" src="https://github.com/user-attachments/assets/fed24269-09b4-41a7-bc1f-b5260f77f51e" />




