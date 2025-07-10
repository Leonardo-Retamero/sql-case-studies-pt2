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

🔎 Principais observações:

• A maioria das regiões apresentou um declínio constante na expectativa de vida ao longo dos três anos.

• A Europa Ocidental foi a única exceção, com uma redução de 2019 para 2020, seguida por um pequeno aumento em 2021, sugerindo uma possível recuperação inicial após os impactos mais severos da pandemia.

• Essa análise destaca a importância de segmentar dados por regiões para compreender variações nos efeitos de eventos globais sobre indicadores de saúde.

🎯 Resultado: 

<img width="423" height="483" alt="image" src="https://github.com/user-attachments/assets/fed24269-09b4-41a7-bc1f-b5260f77f51e" />

---

### 📌 Case 2 – Emissões Totais de CO₂ por Região (2022)

Calcule o total de emissões de dióxido de carbono (CO₂) no ano de 2022, considerando as regiões do mundo segundo a classificação do Banco Mundial. Como os dados disponíveis correspondem às emissões per capita, é necessário multiplicar esse valor pela população de cada país antes de somar. O total deve ser convertido para gigatoneladas (dividindo por 1 bilhão) e o valor final deve ser arredondado para duas casas decimais.

```sql
SELECT c.wb_regions,
	   ROUND(SUM(cep.co2_pc * p.tot_pop) / 1E9, 2) AS tot_giga_co2
FROM co2_emissions_pc cep 
JOIN country c ON cep.country = c.country
JOIN population p ON cep.country = p.country
	AND cep.ref_year = p.ref_year
WHERE cep.ref_year = 2022
GROUP BY c.wb_regions
ORDER BY tot_giga_co2 DESC;
```

📊 Análise:

Essa consulta calcula o total de emissões absolutas de CO₂ (em gigatoneladas) por região no ano de 2022. Como os dados da base são per capita, é feito o cálculo co2_pc * população para obter o total por país, e em seguida os valores são somados por região.

📌 A divisão por 1E9 converte o valor para gigatoneladas (1 bilhão de toneladas), unidade comumente usada para representar emissões globais.

Como esperado, as regiões mais industrializadas apresentam maiores emissões.

🎯 Resultado: 

<img width="324" height="158" alt="image" src="https://github.com/user-attachments/assets/2e3d4fc7-3336-405c-8ae8-4eddbc26c3c1" />

---

### 📌 Case 3 – Regiões com Baixa Mortalidade Infantil (2010)

Identifique, para o ano de 2010, as regiões do mundo segundo a classificação do Banco Mundial que apresentaram uma taxa média de mortalidade infantil considerada baixa, de acordo com os critérios do Ministério da Saúde. Para essa análise, considera-se uma taxa baixa quando o valor médio de mortes infantis é inferior a 20 por mil nascidos vivos.

💻 Código SQL:

```sql
SELECT c.wb_regions,
	   ROUND(AVG(cm.tot_deaths),2) AS avg_tot_deaths
FROM child_mortality cm 
JOIN country c ON cm.country = c.country
WHERE cm.ref_year = 2010
GROUP BY c.wb_regions
HAVING NOT avg_tot_deaths > 20;
```

📊 Análise:

A consulta retorna as regiões que apresentaram, em média, baixas taxas de mortalidade infantil no ano de 2010. A função de agregação AVG é usada para calcular a média regional, e a cláusula HAVING filtra apenas as regiões com valores inferiores a 20 mortes por mil nascidos vivos, conforme o padrão adotado pelo Ministério da Saúde.

🎯 Resultado:

<img width="310" height="67" alt="image" src="https://github.com/user-attachments/assets/70b84c8b-511a-4d8f-817b-54b8aa27375a" />

---

### 📌 Case 4 – Evolução da Expectativa de Vida na América Latina e Caribe (1990–2020)

Compare os valores de expectativa de vida mínima, média e máxima entre os países da América Latina e Caribe ao longo das décadas de 1990, 2000, 2010 e 2020. A análise deve destacar possíveis evoluções no indicador ao longo do tempo e também chamar atenção para eventuais valores fora do padrão.

💻 Código SQL:

```sql
SELECT c.wb_regions,
	   le.ref_year, 
	   MIN(le.tot_years) AS min_expectancy,
	   ROUND(AVG(le.tot_years),2) AS avg_expectancy,
	   MAX(le.tot_years) AS max_expectancy
FROM life_expectancy le 
JOIN country c ON le.country = c.country
WHERE c.wb_regions = 'Latin America & Caribbean'
	AND le.ref_year IN (1990, 2000, 2010, 2020)
GROUP BY c.wb_regions, le.ref_year;
```

📊 Análise:

• Essa consulta permite observar a evolução da expectativa de vida na região da América Latina e Caribe ao longo de quatro décadas, com base nos valores mínimos, médios e máximos registrados a cada ano analisado.

📈 Evolução identificada:

• A média da expectativa de vida apresenta um crescimento constante entre as décadas de 1990 e 2020, indicando avanços consistentes em áreas como saúde pública, nutrição e saneamento.

🔎 Dado que se destaca:

• Em 2010, observa-se um valor mínimo anormalmente baixo, de apenas 35,5 anos, enquanto nas décadas anterior e seguinte os valores mínimos foram 57,4 anos (2000) e 63,6 anos (2020).

• Esse dado pode refletir situações pontuais de crise sanitária, violência ou instabilidade extrema em um país específico naquele período.

🎯 Resultado:

<img width="747" height="100" alt="image" src="https://github.com/user-attachments/assets/96efdc1b-a084-4f7b-be59-d4ebb4d98813" />





